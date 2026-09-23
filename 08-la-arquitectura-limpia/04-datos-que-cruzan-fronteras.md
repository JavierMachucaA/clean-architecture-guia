# 4. Datos que cruzan fronteras

## La regla sobre los datos

Cuando un dato cruza un límite entre anillos, debe hacerlo en la forma **más simple y aislada** posible.

> Lo que cruza las fronteras son **estructuras de datos simples** (DTOs / *request* y *response models*): objetos planos, sin comportamiento ligado a un anillo externo. **Nunca** deben cruzar Entities, ni filas de la base de datos, ni objetos atados a un framework.

```mermaid
flowchart LR
    CTRL["🎮 Controller"] ==>|"📦 request model"| INT{{"⚙️ Interactor"}}
    INT ==>|"📦 response model"| PRES["🖥️ Presenter"]

    style CTRL fill:#37474f,stroke:#90a4ae,stroke-width:2px,color:#fff
    style PRES fill:#37474f,stroke:#90a4ae,stroke-width:2px,color:#fff
    style INT fill:#c62828,stroke:#ff8a80,stroke-width:3px,color:#fff
```

Lo que viaja son **paquetes de datos planos** (📦). En cambio, esto **nunca** debe cruzar la frontera:

```mermaid
flowchart TB
    subgraph OK["✅ SÍ cruzan"]
        direction LR
        D1["📦 DTO / request model"]
        D2["📦 DTO / response model"]
        D3["📦 struct / mapa / tupla"]
    end
    subgraph NO["⛔ NO cruzan"]
        direction LR
        E1["🧠 Entity de negocio"]
        E2["🗄️ fila / row del ORM"]
        E3["🌐 objeto del framework"]
    end

    style OK fill:#1b5e20,stroke:#66bb6a,stroke-width:2px,color:#fff
    style NO fill:#b71c1c,stroke:#ef5350,stroke-width:2px,color:#fff
    style D1 fill:#2e7d32,stroke:#a5d6a7,color:#fff
    style D2 fill:#2e7d32,stroke:#a5d6a7,color:#fff
    style D3 fill:#2e7d32,stroke:#a5d6a7,color:#fff
    style E1 fill:#c62828,stroke:#ffcdd2,color:#fff
    style E2 fill:#c62828,stroke:#ffcdd2,color:#fff
    style E3 fill:#c62828,stroke:#ffcdd2,color:#fff
```

## Por qué no una Entity ni una fila de DB

Si pasas una fila de la base de datos hacia adentro, un anillo interior terminaría **conociendo** un detalle del anillo exterior (la estructura de la tabla). Eso **viola la Regla de Dependencia**: el interior no debe saber nada del exterior.

```mermaid
flowchart LR
    DB[("🗄️ Base de datos")] ==>|"🧱 fila / row"| REPO["🔄 Gateway / Repository<br/><i>traduce aquí</i>"]
    REPO ==>|"📦 DTO simple"| UC["🧠 Use Case<br/>(interior)"]

    DB -. "🚫 la fila NO pasa directo" .-> UC

    style DB fill:#37474f,stroke:#90a4ae,stroke-width:2px,color:#fff
    style REPO fill:#6a1b9a,stroke:#ce93d8,stroke-width:3px,color:#fff
    style UC fill:#c62828,stroke:#ff8a80,stroke-width:2px,color:#fff
    linkStyle 2 stroke:#ff5252,stroke-width:3px,stroke-dasharray:6 4
```

El repositorio **traduce** la fila a una estructura simple antes de entregarla al caso de uso. Así el interior nunca ve el formato del ORM.

## Forma recomendada de los datos que cruzan

```mermaid
flowchart LR
    RM["📥 Request Model<br/>DTO de entrada"] ==> UC{{"⚙️ Use Case<br/>Interactor"}}
    UC ==> RSM["📤 Response Model<br/>DTO de salida"]
    RSM ==> PR["🖥️ Presenter"]

    style RM fill:#1565c0,stroke:#90caf9,stroke-width:2px,color:#fff
    style RSM fill:#1565c0,stroke:#90caf9,stroke-width:2px,color:#fff
    style UC fill:#c62828,stroke:#ff8a80,stroke-width:3px,color:#fff
    style PR fill:#37474f,stroke:#90a4ae,stroke-width:2px,color:#fff
```

Los modelos son **objetos planos** (📥 entra, 📤 sale), inmutables si es posible, sin dependencias hacia frameworks.

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
    A["🚧 Cada frontera define<br/>su propio formato"] ==> B["🔄 Traduce<br/>al cruzar"] ==> C["📦 Entrega una<br/>estructura simple"] ==> D["🔒 El otro lado nunca ve<br/>el formato original"]

    style A fill:#37474f,stroke:#90a4ae,stroke-width:2px,color:#fff
    style B fill:#6a1b9a,stroke:#ce93d8,stroke-width:2px,color:#fff
    style C fill:#1565c0,stroke:#90caf9,stroke-width:2px,color:#fff
    style D fill:#2e7d32,stroke:#a5d6a7,stroke-width:2px,color:#fff
```

De esta forma, un cambio en la base de datos o en el framework se queda contenido en el anillo externo: solo cambia la traducción, no las estructuras que viajan hacia adentro ni las reglas de negocio.

## Punto clave para recordar

> **Por las fronteras solo viajan DTOs y estructuras simples y aisladas.** Nunca Entities ni objetos atados a la base de datos o al framework, porque eso obligaría al interior a conocer al exterior y rompería la Regla de Dependencia.

---

Anterior: [← Cruce de límites e inversión](./03-cruce-de-limites-e-inversion.md) · Volver al índice: [Tópico 8 — La arquitectura limpia](./README.md)
