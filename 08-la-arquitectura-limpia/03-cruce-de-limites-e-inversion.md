# 3. Cruce de límites e inversión

## El aparente conflicto

En algún punto el sistema **tiene que** ir hacia afuera: un caso de uso necesita mostrar algo en pantalla o guardar en la base de datos, que viven en anillos externos. Pero la Regla de Dependencia prohíbe que el interior conozca al exterior. ¿Cómo se resuelve?

> Con **inversión de dependencias**: el **flujo de control** cruza el límite hacia afuera, mientras que la **dependencia del código fuente** apunta hacia adentro.

```mermaid
flowchart LR
    UC["Use Case<br/>(inner)"] -->|"1 · uses"| OP["«interface»<br/>Output Port<br/>(inner)"]
    PG["Presenter / Gateway<br/>(outer)"] -.->|"2 · implements"| OP
    UC -.->|"control flow at runtime"| PG

    style UC fill:#c62828,stroke:#8e0000,stroke-width:2px,color:#fff
    style OP fill:#1565c0,stroke:#0d47a1,stroke-width:2px,color:#fff,stroke-dasharray:5 5
    style PG fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
    linkStyle 2 stroke:#2e7d32,stroke-width:2px,stroke-dasharray:4 4
```

El caso de uso llama a una **interfaz** (un *puerto de salida*) que él mismo declara (flecha 1). El anillo externo la **implementa** (flecha 2). En ejecución el control sale hacia el Presenter (flecha verde), pero la flecha de dependencia del código entra: el Presenter depende del interior, no al revés.

## Puertos de entrada y de salida

```mermaid
flowchart LR
    subgraph EXT["Interface Adapters / Frameworks (outer)"]
        C["Controller"]
        P["Presenter"]
    end

    subgraph INT["Use Cases (inner)"]
        IP["«interface»<br/>Input Port"]
        UC["Use Case<br/>Interactor"]
        OP["«interface»<br/>Output Port"]
    end

    C -->|invokes| IP
    IP --- UC
    UC -->|uses| OP
    P -. implements .-> OP

    style C fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
    style P fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
    style UC fill:#c62828,stroke:#8e0000,stroke-width:2px,color:#fff
    style IP fill:#1565c0,stroke:#0d47a1,stroke-width:2px,color:#fff,stroke-dasharray:5 5
    style OP fill:#1565c0,stroke:#0d47a1,stroke-width:2px,color:#fff,stroke-dasharray:5 5
    style EXT fill:#37474f,stroke:#263238,stroke-width:1px,color:#fff
    style INT fill:#8e0000,stroke:#c62828,stroke-width:1px,color:#fff
```

- El **Controller** depende del **Input Port** (interfaz interior) → dependencia hacia adentro.
- El **Presenter** implementa el **Output Port** (interfaz interior) → la dependencia del Presenter también apunta hacia adentro, aunque el flujo de control salga hacia él.

## Flujo típico: Controller → Use Case → Presenter

1. **Web / Framework → Controller** — empaqueta la petición.
2. **Controller → Input Port** (interface) — entra al caso de uso.
3. **Interactor → Entities** — aplica las reglas de negocio.
4. **Interactor → Output Port** (interface) — entrega el resultado.
5. **Presenter → ViewModel / View** — formatea la salida.

El **control** fluye de 1 a 5 (hacia afuera al final), pero cada **dependencia de código** apunta hacia adentro, a las interfaces del interior. El diagrama de secuencia lo muestra paso a paso:

```mermaid
sequenceDiagram
    participant W as Web [Framework]
    participant C as Controller
    participant U as Use Case Interactor
    participant E as Entities
    participant P as Presenter
    W->>C: HTTP request
    C->>U: request model via Input Port
    U->>E: apply business rules
    E-->>U: result
    U->>P: response model via Output Port
    P-->>W: ViewModel / formatted view
```

## Tabla: control vs. dependencia en el cruce

| Elemento | Dirección del flujo de control | Dirección de la dependencia de código |
|----------|--------------------------------|----------------------------------------|
| Controller → Input Port | hacia adentro | hacia adentro |
| Interactor → Output Port | hacia afuera (conceptualmente) | hacia adentro (interfaz interior) |
| Presenter implementa Output Port | recibe el control desde adentro | hacia adentro (implementa la interfaz interior) |

En todos los casos, **la dependencia de código apunta hacia adentro**, sin importar hacia dónde vaya el control. Este es el mismo truco de la OO: el polimorfismo permite invertir la flecha de dependencia respecto al flujo.

## Punto clave para recordar

> **El control cruza hacia afuera; la dependencia siempre entra.** La inversión de dependencias (puertos de entrada y salida) es lo que permite que un caso de uso "hable" con el exterior sin conocerlo.

---

Anterior: [← La Regla de Dependencia](./02-la-regla-de-dependencia.md) · Siguiente: [Datos que cruzan fronteras →](./04-datos-que-cruzan-fronteras.md)
