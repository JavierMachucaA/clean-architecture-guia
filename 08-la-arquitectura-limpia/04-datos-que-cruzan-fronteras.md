# 4. Datos que cruzan fronteras

## La regla sobre los datos

Cuando un dato cruza un límite entre anillos, debe hacerlo en la forma **más simple y aislada** posible.

> Lo que cruza las fronteras son **estructuras de datos simples** (DTOs / *request* y *response models*): objetos planos, sin comportamiento ligado a un anillo externo. **Nunca** deben cruzar Entities, ni filas de la base de datos, ni objetos atados a un framework.

```mermaid
flowchart LR
    CTRL["Controller"] -->|"request model<br/>(simple struct)"| INT["Interactor"]
    INT -->|"response model<br/>(simple struct)"| PRES["Presenter"]

    FORB["🚫 Prohibido cruzar la frontera con:<br/>· una Entity de negocio<br/>· una fila / row del ORM<br/>· un objeto del framework<br/>(HttpRequest, ResultSet, DataTable...)"]

    style CTRL fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
    style PRES fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
    style INT fill:#c62828,stroke:#8e0000,stroke-width:2px,color:#fff
    style FORB fill:#4a148c,stroke:#6a1b9a,stroke-width:2px,color:#fff
```

## Por qué no una Entity ni una fila de DB

Si pasas una fila de la base de datos hacia adentro, un anillo interior terminaría **conociendo** un detalle del anillo exterior (la estructura de la tabla). Eso **viola la Regla de Dependencia**: el interior no debe saber nada del exterior.

```mermaid
flowchart LR
    DB[("Base de datos")] -->|"fila / row"| REPO["Gateway / Repository<br/>(Interface Adapters)"]
    REPO -->|"DTO simple"| UC["Use Case (interior)"]

    DB -. "NO pasar la fila" .-> UC

    style DB fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
    style REPO fill:#6a1b9a,stroke:#4a148c,stroke-width:2px,color:#fff
    style UC fill:#c62828,stroke:#8e0000,stroke-width:2px,color:#fff
    linkStyle 2 stroke:#e74c3c,stroke-width:2px,stroke-dasharray:5 5
```

El repositorio **traduce** la fila a una estructura simple antes de entregarla al caso de uso. Así el interior nunca ve el formato del ORM.

## Forma recomendada de los datos que cruzan

```mermaid
flowchart TB
    RM["Request Model<br/>(DTO de entrada)"] --> UC["Use Case Interactor"]
    UC --> RSM["Response Model<br/>(DTO de salida)"]
    RSM --> PR["Presenter"]

    style RM fill:#1565c0,stroke:#0d47a1,stroke-width:2px,color:#fff
    style RSM fill:#1565c0,stroke:#0d47a1,stroke-width:2px,color:#fff
    style UC fill:#c62828,stroke:#8e0000,stroke-width:2px,color:#fff
    style PR fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
```

Los modelos son **objetos planos**, inmutables si es posible, sin dependencias hacia frameworks.

## Comparativa: qué cruza y qué no

| ¿Cruza la frontera? | Ejemplo | ¿Permitido? |
|---------------------|---------|-------------|
| DTO / request model | `CreateOrderRequest { itemId, quantity }` | Sí |
| DTO / response model | `OrderResponse { total, status }` | Sí |
| Estructura de datos simple | mapa, registro plano, tupla | Sí |
| Entity de negocio | `Order` con reglas y métodos | No |
| Fila / registro del ORM | `OrderRow`, `ResultSet` | No |
| Objeto del framework | `HttpServletRequest`, `DataTable` | No |

## Regla práctica

```mermaid
flowchart LR
    A["Cada frontera define<br/>su propio formato"] --> B["Traduce al cruzar"] --> C["Entrega una<br/>estructura simple"] --> D["El otro lado nunca ve<br/>el formato original"]

    style A fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
    style B fill:#6a1b9a,stroke:#4a148c,stroke-width:2px,color:#fff
    style C fill:#1565c0,stroke:#0d47a1,stroke-width:2px,color:#fff
    style D fill:#2e7d32,stroke:#1b5e20,stroke-width:2px,color:#fff
```

De esta forma, un cambio en la base de datos o en el framework se queda contenido en el anillo externo: solo cambia la traducción, no las estructuras que viajan hacia adentro ni las reglas de negocio.

## Punto clave para recordar

> **Por las fronteras solo viajan DTOs y estructuras simples y aisladas.** Nunca Entities ni objetos atados a la base de datos o al framework, porque eso obligaría al interior a conocer al exterior y rompería la Regla de Dependencia.

---

Anterior: [← Cruce de límites e inversión](./03-cruce-de-limites-e-inversion.md) · Volver al índice: [Tópico 8 — La arquitectura limpia](./README.md)
