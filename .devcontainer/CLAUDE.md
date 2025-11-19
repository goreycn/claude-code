# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Basic Commands

- `mvn spring-boot:run` - Start Spring Boot application
- `mvn test` - Run all tests
- `mvn clean install` - Clean and install dependencies
- `mvn spring-boot:build-info` - Generate build information

## Code Style Guidelines

- Use Java 17+ language features
- Class names use PascalCase
- Method names use camelCase
- Variable names use descriptive naming
- Controllers return ApiResponse objects directly (HTTP status always 200)
- Entity classes must include toString(), equals() and hashCode() methods
- java file author : Gorey

## API Development Standards

- All API endpoints must include version number (/v1/)
- Use standard HTTP methods:
    * GET - Query
    * POST - Create/UPDATE/DELETE
- Response body must include standard format:
    * Http Status Code: always 200
    * Response JSON body:
        - code: 0:success, xxx: other errors
        - message: "success", or other error messages
        - data: for response data
    * Sample:
        * Success: {"data": {"username":"runner"}, "message": "success", "code": 0}
        * Error: {"data": null, message: "resource not found", "code": 404}


## Dependency Management

- Core dependencies:
    * Spring Web
    * mybatis-plus-spring-boot3-starter
    * Lombok
    * Spring Boot DevTools

## Testing Standards

- Unit test coverage target: 80%
- Integration tests must include:
    * API endpoint testing
    * Database operation testing
    * Error handling testing
- Use @SpringBootTest annotation for integration tests

## Project Structure

- DDD with mybatis

```markdown
feature/
└── order/
    ├── domain/                   # 领域层：核心业务逻辑
    │   ├── model/                # 聚合根与实体
    │   │   ├── Order.java
    │   │   ├── OrderItem.java
    │   │   └── PaymentRecord.java
    │   ├── repository/           # 仓储接口（Domain Port）
    │   │   ├── OrderRepository.java
    │   │   ├── OrderItemRepository.java
    │   │   └── PaymentRecordRepository.java
    │   └── service/              # 领域服务（纯逻辑）
    │       └── OrderDomainService.java
    │
    ├── application/              # 应用层（用例服务）
    │   └── service/
    │       └── OrderAppService.java
    │
    ├── presentation/             # 接口层（Controller）
    │   ├── rest/
    │   │   └── OrderController.java
    │   └── dto/
    │       ├── OrderRequest.java
    │       └── OrderResponse.java
    │
    └── infrastructure/           # 基础设施层（技术实现）
        ├── persistence/
        │   ├── mapper/           # MyBatis 接口（Mapper）
        │   │   ├── OrderMapper.java
        │   │   ├── OrderItemMapper.java
        │   │   └── PaymentRecordMapper.java
        │   ├── adapter/          # Repository 的实现类
        │   │   ├── OrderRepositoryImpl.java
        │   │   ├── OrderItemRepositoryImpl.java
        │   │   └── PaymentRecordRepositoryImpl.java
        │   └── converter/        # entity <-> model 转换
        │       ├── OrderConverter.java
        │       └── PaymentConverter.java
        │
        └── config/
            └── MyBatisConfig.java
```

## Common Issues and Solutions

- Lombok annotations not working:
    * Check IDE Lombok plugin installation
    * Verify pom.xml Lombok dependency
    * Clean and recompile project
