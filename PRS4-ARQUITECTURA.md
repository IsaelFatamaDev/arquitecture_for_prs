# PRS4 · Arquitectura

![PRS4: comunicación animada entre capas (hexagonal)](./svg/PRS4-COMUNICACION-CAPAS.svg)

## Estructura hexagonal

## Árbol completo de carpetas y paquetes

Raíz canónica: `pe.edu.vallegrande.sigrc.<contexto>`.

```text
<microservice>/
├── pom.xml
├── Dockerfile
├── README.md
├── .env.example
├── src/
│   ├── main/
│   │   ├── java/pe/edu/vallegrande/sigrc/<contexto>/
│   │   │   ├── <Contexto>Application.java                 [class]
│   │   │   ├── domain/                                    [package: Java puro]
│   │   │   │   ├── model/                                 [class y enum]
│   │   │   │   │   ├── <Aggregate>.java                  [class]
│   │   │   │   │   └── <Status>.java                     [enum]
│   │   │   │   ├── valueobject/                           [record]
│   │   │   │   │   ├── <Aggregate>Id.java                [record]
│   │   │   │   │   └── OrganizationId.java               [record]
│   │   │   │   └── exception/                             [exception]
│   │   │   │       └── <BusinessRule>Exception.java       [exception]
│   │   │   ├── application/
│   │   │   │   ├── port/
│   │   │   │   │   ├── in/                                [interface]
│   │   │   │   │   │   └── <Verb><Aggregate>UseCase.java [interface]
│   │   │   │   │   └── out/                               [interface]
│   │   │   │   │       ├── <Aggregate>RepositoryPort.java [interface]
│   │   │   │   │       └── <Remote>ClientPort.java        [interface]
│   │   │   │   ├── usecase/                               [class]
│   │   │   │   │   └── <Verb><Aggregate>UseCaseImpl.java  [class]
│   │   │   │   ├── dto/
│   │   │   │   │   ├── input/                             [record]
│   │   │   │   │   └── output/                            [record]
│   │   │   │   ├── context/
│   │   │   │   │   └── TenantContext.java                 [record]
│   │   │   │   └── exception/
│   │   │   │       └── ApplicationException.java          [exception]
│   │   │   └── infrastructure/
│   │   │       ├── adapter/
│   │   │       │   ├── in/
│   │   │       │   │   └── rest/
│   │   │       │   │       ├── <Aggregate>Controller.java [class]
│   │   │       │   │       ├── dto/
│   │   │       │   │       │   ├── request/               [record]
│   │   │       │   │       │   └── response/              [record]
│   │   │       │   │       └── mapper/                    [interface]
│   │   │       │   └── out/
│   │   │       │       ├── persistence/
│   │   │       │       │   ├── postgres/
│   │   │       │       │   │   ├── entity/                [class]
│   │   │       │       │   │   ├── repository/            [interface]
│   │   │       │       │   │   └── mapper/                [interface]
│   │   │       │       │   └── mongo/
│   │   │       │       │       ├── document/              [class]
│   │   │       │       │       ├── repository/            [interface]
│   │   │       │       │       └── mapper/                [interface]
│   │   │       │       └── http/
│   │   │       │           └── <remote>/
│   │   │       │               ├── dto/
│   │   │       │               │   ├── request/           [record]
│   │   │       │               │   └── response/          [record]
│   │   │       │               ├── mapper/                [interface]
│   │   │       │               └── <Remote>WebClientAdapter.java [class]
│   │   │       ├── config/                                 [configuration class]
│   │   │       ├── security/                               [configuration y class]
│   │   │       └── exception/                              [handler y enum]
│   │   └── resources/
│   │       ├── application.yml
│   │       ├── application-dev.yml
│   │       └── application-prod.yml
│   └── test/
│       └── java/pe/edu/vallegrande/sigrc/<contexto>/
│           ├── domain/                                     [test class]
│           ├── application/                                [test class]
│           └── infrastructure/                             [test class]
```

## Capa de aplicación e integraciones HTTP

```text
application/
├── port/in/
├── port/out/
├── usecase/
├── dto/input/
├── dto/output/
└── exception/

infrastructure/adapter/out/http/<remote>/
├── config/
├── dto/request/
├── dto/response/
├── mapper/
├── exception/
└── <Remote>WebClientAdapter.java
```

## Tenant, comunicación y datos

### Tree de multitenancy

```text
<contexto>/
├── domain/valueobject/OrganizationId.java
├── application/context/TenantContext.java
└── infrastructure/
    ├── security/JwtTenantClaimResolver.java
    ├── security/TenantAccessValidator.java
    ├── web/TenantContextWebFilter.java
    └── persistence/TenantCriteria.java
```

## Árbol especial del gateway

```text
pe/edu/vallegrande/sigrc/gateway/
├── GatewayApplication.java
└── infrastructure/
    ├── config/
    ├── filter/
    ├── security/
    └── exception/
```
