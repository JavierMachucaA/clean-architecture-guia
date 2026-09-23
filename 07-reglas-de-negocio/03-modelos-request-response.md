# 3. Modelos de request y response

## Qué son

> Los **modelos de request y response** son estructuras de datos **planas y simples** que un caso de uso recibe como entrada y produce como salida. No contienen lógica, no conocen HTTP y no conocen la base de datos.

Su única misión es transportar datos a través del **boundary** (límite) que rodea al caso de uso: del lado de afuera (controllers, UI) hacia adentro (use case) y de vuelta (use case hacia presenters).

## Por qué existen (y no se reutiliza la entidad)

Podría parecer natural pasarle la entidad directamente al controller o devolverla en la respuesta. Uncle Bob lo desaconseja: crearía un **acoplamiento** entre el caso de uso y su entorno.

```mermaid
flowchart LR
    subgraph bad["❌ Acoplamiento fuerte"]
        C1["Controller"] --> E1["Entity"]
    end
    subgraph good["✅ Boundary limpio"]
        C2["Controller"] --> RQ["RequestModel"] --> UC["UseCase"]
        UC --> RS["ResponseModel"] --> PR["Presenter"]
    end

    style C1 fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
    style E1 fill:#c62828,stroke:#8e0000,stroke-width:2px,color:#fff
    style C2 fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
    style PR fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
    style RQ fill:#1565c0,stroke:#0d47a1,stroke-width:2px,color:#fff
    style RS fill:#1565c0,stroke:#0d47a1,stroke-width:2px,color:#fff
    style UC fill:#2e7d32,stroke:#1b5e20,stroke-width:2px,color:#fff
    style bad fill:#8e0000,stroke:#c62828,stroke-width:1px,color:#fff
    style good fill:#1b5e20,stroke:#2e7d32,stroke-width:1px,color:#fff
```

En el lado ❌ el controller depende de la estructura interna de la regla de negocio. En el lado ✅ nadie depende de la entidad interna: los modelos son el contrato del borde.

Los modelos son un **contrato estable** en el borde del caso de uso, desacoplado de cómo estén hechas las entidades por dentro.

## Estructura: solo datos

```mermaid
classDiagram
    class ApproveLoanRequest {
        +String applicantId
        +int score
        +double requestedAmount
        +int termMonths
    }
    class ApproveLoanResponse {
        +boolean approved
        +double monthlyPayment
        +String rejectionReason
    }
    class ApproveLoanUseCase {
        +execute(ApproveLoanRequest) ApproveLoanResponse
    }
    ApproveLoanUseCase ..> ApproveLoanRequest : receives
    ApproveLoanUseCase ..> ApproveLoanResponse : produces
```

## Flujo del dato a través del boundary

```mermaid
flowchart TB
    WEB["Web / HTTP"] -->|parsea JSON| RQ["RequestModel<br/>plano: strings, números, fechas"]
    RQ --> UC["USE CASE<br/>usa entidades, aplica reglas"]
    UC --> RS["ResponseModel<br/>plano: sin objetos de dominio"]
    RS --> PR["Presenter → HTML / JSON / UI"]

    style WEB fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
    style PR fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
    style RQ fill:#1565c0,stroke:#0d47a1,stroke-width:2px,color:#fff
    style RS fill:#1565c0,stroke:#0d47a1,stroke-width:2px,color:#fff
    style UC fill:#2e7d32,stroke:#1b5e20,stroke-width:2px,color:#fff
```

## Reglas de un buen modelo

| Regla | Motivo |
|-------|--------|
| Solo tipos simples y colecciones | Evita arrastrar dependencias |
| Sin anotaciones de framework web | No debe saber de HTTP |
| Sin anotaciones de ORM/DB | No debe saber de persistencia |
| Sin lógica de negocio | Es un contenedor de datos, no una regla |
| No contiene la entidad | Rompería el aislamiento del núcleo |

### Ejemplo ❌ — request atado a la web y a la entidad

```java
// ❌ Knows about HTTP (annotations) and exposes the domain entity
class ApproveLoanRequest {
    @JsonProperty("amount") HttpServletRequest raw;
    Loan domainLoan; // leaks the entity outward
}
```

### Ejemplo ✅ — request plano y neutral

```java
// ✅ Only simple data; knows nothing about HTTP or the DB
class ApproveLoanRequest {
    final String applicantId;
    final int score;
    final double requestedAmount;
    final int termMonths;
}
```

## Punto clave para recordar

> **Los modelos de request y response son estructuras de datos planas que cruzan el boundary del caso de uso.** No conocen HTTP ni la base de datos, y nunca deben ser reemplazados por las entidades para evitar acoplar el núcleo con el exterior.

---

Anterior: [← Use Cases: reglas de aplicación](./02-use-cases-reglas-de-aplicacion.md) · Siguiente: [Relación entities ↔ use cases →](./04-relacion-entities-usecases.md)
