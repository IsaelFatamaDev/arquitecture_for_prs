# PRS5 · Arquitectura

![PRS5: comunicación animada entre capas (hexagonal)](./svg/PRS5-COMUNICACION-CAPAS.svg)

## Estructura hexagonal

## Árbol completo de carpetas y paquetes

Raíz canónica: `pe.edu.vallegrande.edunova.<contexto>`.

```text
<microservice>/
├── pom.xml
├── Dockerfile
├── README.md
├── .env.example
├── src/main/java/pe/edu/vallegrande/edunova/<contexto>/
│   ├── <Contexto>Application.java [class]
│   ├── domain/                    [package: clases puras]
│   │   ├── model/                 [class y enum]
│   │   │   ├── <Aggregate>.java   [class]
│   │   │   └── <Status>.java      [enum]
│   │   ├── valueobject/           [record]
│   │   │   ├── <Aggregate>Id.java [record]
│   │   │   └── TenantId.java      [record]
│   │   └── exception/             [exception]
│   │       └── <BusinessRule>Exception.java [exception]
│   ├── application/               [casos de uso y puertos]
│   │   ├── port/
│   │   │   ├── in/                [interface]
│   │   │   │   └── <Verb><Aggregate>UseCase.java [interface]
│   │   │   └── out/               [interface]
│   │   │       ├── <Aggregate>RepositoryPort.java [interface]
│   │   │       └── <Remote>ClientPort.java [interface]
│   │   ├── usecase/               [class]
│   │   │   └── <Verb><Aggregate>UseCaseImpl.java [class]
│   │   ├── dto/
│   │   │   ├── input/             [record]
│   │   │   └── output/            [record]
│   │   ├── context/               [record]
│   │   │   └── TenantContext.java [record]
│   │   └── exception/             [exception]
│   │       └── ApplicationException.java [exception]
│   └── infrastructure/            [adaptadores Spring]
│       ├── adapter/
│       │   ├── in/
│       │   │   └── rest/
│       │   │       ├── <Aggregate>Controller.java [class]
│       │   │       ├── dto/
│       │   │       │   ├── request/  [record]
│       │   │       │   └── response/ [record]
│       │   │       └── mapper/       [interface]
│       │   └── out/
│       │       ├── persistence/
│       │       │   └── postgres/
│       │       │       ├── entity/     [class]
│       │       │       ├── repository/ [interface]
│       │       │       └── mapper/     [interface]
│       │       └── http/
│       │           └── <remote>/
│       │               ├── dto/
│       │               │   ├── request/  [record]
│       │               │   └── response/ [record]
│       │               ├── mapper/       [interface]
│       │               └── <Remote>WebClientAdapter.java [class]
│       ├── config/        [configuration class]
│       ├── security/      [configuration y class]
│       └── exception/     [exception handler y enum]
├── src/main/resources/
│   ├── application.yml
│   ├── application-dev.yml
│   └── application-prod.yml
├── src/test/
│       └── java/
│           └── pe/edu/vallegrande/edunova/<contexto>/
│               ├── domain/               [test class]
│               ├── application/          [test class]
│               └── infrastructure/       [test class]
```

## Capa de aplicación e integraciones HTTP

```text
application/
├── port/in/                                  [interfaces]
├── port/out/                                 [interfaces]
├── usecase/                                  [classes]
├── dto/input/                                [records]
├── dto/output/                               [records]
└── exception/                                [exceptions]

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
├── domain/valueobject/TenantId.java
├── application/context/TenantContext.java
└── infrastructure/
    ├── security/JwtTenantClaimResolver.java
    ├── security/TenantAccessValidator.java
    ├── web/TenantContextWebFilter.java
    └── persistence/TenantCriteria.java
```

## Árbol especial del gateway

```text
pe/edu/vallegrande/edunova/gateway/
├── GatewayApplication.java
└── infrastructure/
    ├── config/
    ├── filter/
    ├── security/
    └── exception/
```
