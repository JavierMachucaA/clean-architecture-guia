# 2. La Regla de Dependencia

## El enunciado

> **La Regla de Dependencia:** las dependencias del código fuente deben apuntar **solo hacia adentro**, hacia políticas de más alto nivel.

Dicho de otro modo: **nada en un anillo interior puede saber absolutamente nada de algo en un anillo exterior**. Los nombres declarados en un anillo externo (una función, una clase, una variable, una entidad de software) **no deben ser mencionados** por el código de un anillo interno.

```mermaid
flowchart LR
    FD["Frameworks & Drivers<br/>(exterior)"] -->|depende de| IA["Interface Adapters"]
    IA -->|depende de| UC["Use Cases"]
    UC -->|depende de| EN["Entities<br/>(interior)"]

    style FD fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
    style IA fill:#6a1b9a,stroke:#4a148c,stroke-width:2px,color:#fff
    style UC fill:#c62828,stroke:#8e0000,stroke-width:2px,color:#fff
    style EN fill:#f9a825,stroke:#f57f17,stroke-width:2px,color:#000
```

La flecha significa "depende de / conoce a". Toda flecha apunta **hacia adentro**; ninguna apunta hacia afuera.

## Lo interior no conoce lo exterior

El centro no sabe que existe el borde. Las Entities no saben que hay casos de uso; los casos de uso no saben qué base de datos o qué framework web se usa.

```mermaid
flowchart LR
    FD["Frameworks & Drivers"] --> IA["Interface Adapters"]
    IA --> UC["Use Cases"]
    UC --> EN["Entities"]

    EN -. NO conoce .-> UC
    UC -. NO conoce .-> IA
    IA -. NO conoce .-> FD

    style FD fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
    style IA fill:#6a1b9a,stroke:#4a148c,stroke-width:2px,color:#fff
    style UC fill:#c62828,stroke:#8e0000,stroke-width:2px,color:#fff
    style EN fill:#f9a825,stroke:#f57f17,stroke-width:2px,color:#000
    linkStyle 3,4,5 stroke:#e74c3c,stroke-width:2px,stroke-dasharray:5 5
```

Las flechas continuas (dependencias reales) van hacia adentro. Las flechas punteadas rojas representan un "conocimiento hacia afuera" que **está prohibido**.

## Qué NO debe cruzar hacia adentro

| Elemento del anillo externo | ¿Puede mencionarse en un anillo interno? |
|-----------------------------|-------------------------------------------|
| Nombre de una clase de framework (p. ej. `HttpServletRequest`) | No |
| Tipos del ORM o filas de la base de datos | No |
| Formatos de la UI (JSON, HTML, widgets) | No |
| Una función/variable declarada en Interface Adapters | No (desde Use Cases o Entities) |
| Una abstracción/puerto declarado en el interior | Sí — el exterior sí puede depender de ella |

## Independencia que produce la regla

Cuando el código respeta la Regla de Dependencia, el núcleo del sistema queda **independiente de los detalles**:

```mermaid
flowchart TD
    R["Regla de Dependencia respetada"] --> A["Independiente de frameworks"]
    R --> B["Independiente de la UI"]
    R --> C["Independiente de la base de datos"]
    R --> D["Independiente de agentes externos"]
    A --> T["Reglas de negocio comprobables<br/>sin UI, DB ni servidor"]
    B --> T
    C --> T
    D --> T

    style R fill:#1565c0,stroke:#0d47a1,stroke-width:2px,color:#fff
    style T fill:#2e7d32,stroke:#1b5e20,stroke-width:2px,color:#fff
```

Por eso puedes cambiar de base de datos (de Oracle a SQL Server), de framework web o de UI **sin tocar** las reglas de negocio: esos cambios ocurren en anillos externos y la regla impide que el interior dependa de ellos.

## Punto clave para recordar

> **El código fuente solo apunta hacia adentro.** Lo interior (estable, valioso) jamás menciona nombres de lo exterior (volátil, reemplazable). Esa es la única regla que hace "limpia" a la arquitectura.

---

Anterior: [← Los cuatro anillos](./01-los-cuatro-anillos.md) · Siguiente: [Cruce de límites e inversión →](./03-cruce-de-limites-e-inversion.md)
