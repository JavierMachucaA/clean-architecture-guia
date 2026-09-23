# 5. La lucha por la arquitectura frente a la presión por "sacar features"

## Por qué es una lucha

Defender la arquitectura no es una tarea pacífica: es un **conflicto permanente** entre dos fuerzas dentro de la organización.

```mermaid
flowchart LR
    subgraph Negocio["Presión del negocio"]
        F["'We need the feature NOW!'"]
        U["Constant urgency"]
    end
    subgraph Devs["Equipo de desarrollo"]
        A["Protect the structure"]
        Q["Keep the software 'soft'"]
    end

    Negocio <==>|tension| Devs

    style F fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style U fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style A fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style Q fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
```

El negocio empuja por **comportamiento urgente**; el equipo debe empujar por **estructura sostenible**. Si una fuerza gana siempre, el sistema pierde.

## El error del equipo pasivo

Muchos equipos ceden por completo a la presión de features:

```mermaid
flowchart TD
    P["⚙️ Feature pressure"] ==> C["🚫 The team always<br/>gives in to urgency"]
    C ==> D["Architecture degrades"]
    D ==> S["Development becomes slow"]
    S ==> P2["Even more pressure"]
    P2 ==> C

    style P fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style C fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style D fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style S fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style P2 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
```

Este ciclo (el mismo del caso de estudio) termina con un sistema paralizado. Al ceder "para ayudar al negocio", el equipo en realidad **daña al negocio** a mediano plazo.

## La postura correcta: defender la estructura

Uncle Bob es enfático: **el equipo de desarrollo debe luchar por la arquitectura como lo hace por cualquier otro requisito.**

Analogías que usa el libro:

| Rol | Comportamiento esperado |
|-----|-------------------------|
| Equipo de gestión | Lucha por el cronograma y el presupuesto. |
| Equipo de marketing | Lucha por sus objetivos de mercado. |
| Equipo de operaciones | Lucha por la estabilidad y la operación. |
| **Equipo de desarrollo** | **Debe luchar por la arquitectura con la misma firmeza.** |

> Si eres desarrollador de software, **tú** eres un stakeholder. Tienes que luchar por lo que sabes que el sistema necesita para sobrevivir. Ese es tu rol y tu deber.

## No es una cuestión de "pedir permiso"

La arquitectura no debe negociarse como si fuera un lujo opcional. Es parte del trabajo, igual que la seguridad o la corrección:

```mermaid
flowchart TB
    WRONG["🚫 'Will you give us time<br/>to do good architecture?'"]
    RIGHT["✅ Good architecture is part of<br/>doing the job well,<br/>not an extra you ask for apart"]

    style WRONG fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style RIGHT fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:3px
```

Un cirujano no pide permiso para lavarse las manos; forma parte de operar bien. Del mismo modo, la estructura sana es parte de programar bien.

## El equilibrio, no el extremo

Defender la arquitectura **no** significa ignorar al negocio ni caer en el *gold plating* (sobre-ingeniería). Significa mantener la tensión sana:

```mermaid
flowchart LR
    E1["🚫 Features only<br/>(no structure)"] ==>|imbalance| X1["Paralyzed system"]
    E2["🚫 Architecture only<br/>(no value delivered)"] ==>|imbalance| X2["Irrelevant project"]
    BAL["Balance:<br/>deliver value TODAY<br/>without mortgaging TOMORROW"] ==> OK["✅ Healthy, living system"]

    style E1 fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style E2 fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style X1 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style X2 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style BAL fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:3px
    style OK fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
```

El objetivo es entregar el comportamiento que el negocio necesita **sin** destruir la capacidad de cambiar el sistema en el futuro.

## Resumen del tópico completo

```mermaid
flowchart TD
    T1["Design = Architecture<br/>(a single fabric)"] ==> T2["Goal: minimize<br/>human effort"]
    T2 ==> T3["Case study:<br/>disorder slows productivity"]
    T3 ==> T4["Dilemma: behavior (urgent)<br/>vs structure (important)"]
    T4 ==> T5["The fight: the dev MUST<br/>defend the architecture"]
    T5 ==> R["✅ Software that stays<br/>cheap to change over time"]

    style T1 fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    style T2 fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    style T3 fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style T4 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style T5 fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style R fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:3px
```

## Punto clave para recordar

> **La arquitectura no se regala: se defiende.** El equipo de desarrollo es un stakeholder más y su deber es luchar por la estructura del sistema con la misma firmeza con que otros luchan por sus objetivos, buscando el equilibrio entre entregar hoy y poder cambiar mañana.

---

Anterior: [← Comportamiento vs. Estructura](./04-comportamiento-vs-estructura.md) · Volver al [índice del tópico](./README.md)
