# 2. El objetivo de la arquitectura

## La definición de Uncle Bob

> **El objetivo de la arquitectura de software es minimizar los recursos humanos necesarios para construir y mantener el sistema requerido.**

Fíjate en lo que *no* dice:

- No dice "hacer el sistema más rápido".
- No dice "usar la tecnología más moderna".
- No dice "que sea elegante".

El criterio es **económico y humano**: cuánto esfuerzo (personas × tiempo × dinero) cuesta entregar el comportamiento que el negocio necesita, hoy y en el futuro.

## Comportamiento vs. costo del comportamiento

Todo sistema tiene dos valores:

```mermaid
flowchart TD
    S["🖥️ Software system"] ==> B["VALUE 1: Behavior<br/>(what it does today)"]
    S ==> A["VALUE 2: Architecture<br/>(how cheap it is to change tomorrow)"]

    B ==> Bnote["Urgent and visible<br/>stakeholders ask for it"]
    A ==> Anote["Important but invisible<br/>almost nobody asks for it"]

    style S fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:3px
    style B fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style A fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style Bnote fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style Anote fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
```

La buena arquitectura ataca el **VALOR 2**: hace que el costo de cada nuevo cambio se mantenga **bajo y estable** a lo largo de la vida del sistema.

## La señal de una buena vs. mala arquitectura

La medida no es una foto de un instante, sino la **tendencia del costo por feature**:

```mermaid
flowchart LR
    T["⏳ Time / number of features"] ==> BAD["📈 Bad architecture<br/>(cost per feature keeps rising)"]
    T ==> GOOD["➖ Good architecture<br/>(stable cost)"]

    style T fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    style BAD fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style GOOD fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:3px
```

- **Mala arquitectura:** al inicio se avanza rápido, pero el costo por cada nueva funcionalidad crece hasta que el equipo casi no puede entregar nada.
- **Buena arquitectura:** el costo por funcionalidad se mantiene plano; el sistema sigue siendo "blando" (fácil de cambiar), que es literalmente el sentido de *soft*ware.

## El error de "primero rápido, luego limpiamos"

Uncle Bob desmonta el mito de que ir sucio al principio te hace ir más rápido:

```mermaid
flowchart LR
    subgraph Mito["El mito"]
        M1["🚫 Going dirty<br/>= going fast"] ==> M2["We'll clean up<br/>later"]
    end
    subgraph Realidad["La realidad"]
        R1["⛔ Going dirty slows you<br/>almost immediately"] ==> R2["'Later'<br/>never comes"]
    end

    style M1 fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style M2 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style R1 fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style R2 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
```

El código desordenado te ralentiza **el mismo día**, no en un futuro lejano. La única forma de ir rápido es ir bien (*the only way to go fast is to go well*).

## Resumen

| Pregunta | Respuesta de Clean Architecture |
|----------|--------------------------------|
| ¿Qué mide una buena arquitectura? | El esfuerzo humano por cada cambio a lo largo del tiempo. |
| ¿Qué valor protege? | El VALOR 2: la capacidad de cambiar barato (no solo el comportamiento de hoy). |
| ¿Ir sucio es más rápido? | No. Te frena de inmediato; ir bien es la única forma de ir rápido. |

## Punto clave para recordar

> **Una buena arquitectura mantiene bajo y constante el costo de cambiar el software.** Su métrica no es el rendimiento ni la estética, sino el esfuerzo humano en el tiempo.

---

Anterior: [← Diseño vs. Arquitectura](./01-diseno-vs-arquitectura.md) · Siguiente: [Caso de estudio: productividad →](./03-caso-de-estudio-productividad.md)
