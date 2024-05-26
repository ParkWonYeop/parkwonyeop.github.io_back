---
layout: post
read_time: true
show_date: true
title: Writer&Reader(Master&Slave)
img: ":title.png"
date: "2024-05-15 18:23:20"
description: 
tags:
  - Spring
author: ParkWonyeop
published: true
category: Spring
---
## 서론

데이터베이스 서버를 구축할 때 하나의 서버로만 운영한다면 어떻게 될까?  
수 많은 요청이 들어왔을때 데이터 전달이 지연되는 상황이 생길 수 밖에 없을 것이다.  
오늘은 서버의 부담을 줄이기 위한 구조인 Master-Slave 방식에 대해서 알아보자.  

## Master-Slave

Master-Slave 방식이지만 여러가지 이유와 개인적인 사유로 Writer-Reader라고 바꿔서 부르도록 하겠다.  
단어 자체가 주인과 노예를 뜻하는 것이기때문에 사용을 지양하는 것이 좋다는 판단을 했다.  

간단하다면 간단한 방식인데, 주요한 데이터 처리를 담당하는 서버와 단순 조회와 같은 처리를 담당하는 서브 서버로 데이터베이스 서버를 구축하는 것이다.  
Writer는 데이터를 저장하고, 수정하는 중요한 처리를 담당하는데 이러한 처리가 지연될 경우 동시성 제어면에서 매우 안좋기때문에 이러한 지연을 최대한 줄이고자 이런 구조를 만드는 것이다.  

그렇다면 Reader의 역할은 단순히 데이터를 조회하는 것과 같이 간단한 요청을 처리한다.  

## 구현

이론은 단순한데 구현은 상당히 복잡하다.  
오늘 구현하는 방식은 Transaction어노테이션의 readonly 값에 따라 reader와 writer로 나눠 처리하는 방식에 대해서 알아볼 것이다.  

```yml
spring:
  datasource:
    writer:
      hikari:
        url: jdbc:mysql://localhost:3306/reservation
        username: writer
        password: password
    reader:
      hikari:
        url: jdbc:mysql://localhost:3307/reservation
        username: reader
        password: password
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    hibernate:
      ddl-auto: update
    defer-datasource-initialization: true
```

데이터베이스에 대한 설정을 위와 같이 해준다.  
writer와 reader에 대한 설정 값을 입력해주면 된다.  

```kotlin
@Component
class DataBaseProperty(
    @Value("\${spring.datasource.writer.hikari.url}")
    val writerUrl: String,
    @Value("\${spring.datasource.writer.hikari.username}")
    val writerUsername: String,
    @Value("\${spring.datasource.writer.hikari.password}")
    var writerPassword: String,
    @Value("\${spring.datasource.reader.hikari.url}")
    val readerUrl: String,
    @Value("\${spring.datasource.reader.hikari.username}")
    val readerUsername: String,
    @Value("\${spring.datasource.reader.hikari.password}")
    val readerPassword: String,
    @Value("\${spring.datasource.driver-class-name}")
    val driverClassName: String
)
```

위와 같이 데이터베이스 설정값을 하나의 클래스에 담아준다.  

```kotlin
class RoutingDataSource : AbstractRoutingDataSource() {
    override fun determineCurrentLookupKey(): String {
        val isReadOnly = TransactionSynchronizationManager.isCurrentTransactionReadOnly()
        return if (isReadOnly) "reader"
        else "writer"
    }
}
```

다음으로 현재 Transaction 어노테이션의 readonly 값에 대해서 받아주는 코드를 작성한다.  

```kotlin
@Configuration
class DataSourceConfiguration(private val dataBaseProperty: DataBaseProperty) {
    @Bean
    fun writerDataSource(): HikariDataSource {
        val hikariDataSource = HikariDataSource()
        hikariDataSource.driverClassName = dataBaseProperty.driverClassName
        hikariDataSource.jdbcUrl = dataBaseProperty.writerUrl
        hikariDataSource.username = dataBaseProperty.writerUsername
        hikariDataSource.password = dataBaseProperty.writerPassword
        return hikariDataSource
    }

    @Bean
    fun readerDataSource(): HikariDataSource {
        val hikariDataSource = HikariDataSource()
        hikariDataSource.driverClassName = dataBaseProperty.driverClassName
        hikariDataSource.jdbcUrl = dataBaseProperty.readerUrl
        hikariDataSource.username = dataBaseProperty.readerUsername
        hikariDataSource.password = dataBaseProperty.readerPassword
        return hikariDataSource
    }


    @Bean
    fun routingDataSource(
        @Qualifier("writerDataSource") writerDataSource: DataSource,
        @Qualifier("readerDataSource") readerDataSource: DataSource,
    ): DataSource {
        val routingDataSource = RoutingDataSource()

        val datasourceMap = HashMap<Any, Any>()

        datasourceMap["writer"] = writerDataSource
        datasourceMap["reader"] = readerDataSource

        routingDataSource.setTargetDataSources(datasourceMap)

        routingDataSource.setDefaultTargetDataSource(writerDataSource)

        return routingDataSource
    }

    @Primary
    @Bean
    fun dataSource(
        @Qualifier("routingDataSource") routingDataSource: DataSource,
    ) = LazyConnectionDataSourceProxy(routingDataSource)
}
```

해당 코드는 reader와 wirter의 데이터소스를 Bean에 등록하고 아까 위의 readonly 값에 따라 writer와 reader로 바꾸는 역할을 한다.  

```kotlin
@Configuration
@EnableTransactionManagement
class JpaConfiguration(
    @Value("\${spring.jpa.hibernate.ddl-auto}")
    private val auto: String,
) {
    @Bean
    fun entityManagerFactory(
        @Qualifier("dataSource") dataSource: DataSource,
    ): LocalContainerEntityManagerFactoryBean {
        val entityManagerFactoryBean = LocalContainerEntityManagerFactoryBean()
        entityManagerFactoryBean.dataSource = dataSource
        entityManagerFactoryBean.setPackagesToScan("com.example.libraryReservationKotlin")
        entityManagerFactoryBean.jpaVendorAdapter = jpaVendorAdapter()
        entityManagerFactoryBean.persistenceUnitName = "entityManager"
        entityManagerFactoryBean.setJpaProperties(hibernateProperties())
        return entityManagerFactoryBean
    }

    private fun jpaVendorAdapter(): JpaVendorAdapter {
        val hibernateJpaVendorAdapter = HibernateJpaVendorAdapter()
        hibernateJpaVendorAdapter.setShowSql(false)
        hibernateJpaVendorAdapter.setDatabase(Database.MYSQL)
        hibernateJpaVendorAdapter.setDatabasePlatform("org.hibernate.dialect.MySQLDialect")
        return hibernateJpaVendorAdapter
    }

    private fun hibernateProperties(): Properties {
        val properties = Properties()
        properties.setProperty("hibernate.hbm2ddl.auto", auto)
        properties.setProperty("hibernate.format_sql", "true")
        return properties
    }

    @Bean
    fun transactionManager(
        @Qualifier("entityManagerFactory")
        entityManagerFactory: LocalContainerEntityManagerFactoryBean,
    ): JpaTransactionManager {
        val jpaTransactionManager = JpaTransactionManager()
        jpaTransactionManager.entityManagerFactory = entityManagerFactory.`object`
        return jpaTransactionManager
    }
}
```

마지막으로 JPA까지 설정해주면 모든 설정은 끝난다.  

이렇게 설정해주면 Transaction의 readonly값에 따라 reader와 writer로 나뉘어서 작동할 것이다.  

## 마치며

이 방식 외에도 다른 방식도 존재하는데 다음에는 그 방식으로 구현해보도록 하겠다.  