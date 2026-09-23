# 4. El dilema de los desarrolladores: comportamiento vs. estructura

## Los dos valores del software

Todo sistema de software aporta a sus stakeholders **dos valores distintos**:

```mermaid
flowchart LR
    SW["🖥️ Software"] ==> C["Behavior"]
    SW ==> E["Architecture"]

    C ==> C1["Make the machine<br/>do what the business asks"]
    C ==> C2["Fix behavior<br/>bugs"]

    E ==> E1["Keep the software<br/>easy to change (soft)"]
    E ==> E2["Let new requirements<br/>be implemented cheaply"]

    style SW fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:3px
    style C fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style E fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style C1 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style C2 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style E1 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style E2 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
```

1. **Comportamiento:** lo que el sistema hace. Es urgente, visible y lo que todos piden. "Que funcione".
2. **Estructura:** la capacidad de que el software siga siendo *blando* (fácil de cambiar). Es importante pero invisible; casi nadie lo pide de forma explícita.

## La palabra "software"

Uncle Bob juega con la etimología: *soft-ware* significa "producto blando". La razón de existir del software (frente al *hardware*) es precisamente que se puede **cambiar con facilidad**. Si un sistema es difícil de cambiar, ha traicionado su propia naturaleza.

> El valor de una funcionalidad no está solo en que funcione hoy, sino en poder **cambiarla** cuando el negocio cambie.

## La matriz de Eisenhower aplicada

Aquí aparece la trampa. El dilema se entiende con la matriz **urgente / importante**:

```mermaid
flowchart TB
    subgraph Urgent["URGENTE"]
        Q1["1 — Critical behavior<br/>(important, do it now)"]
        Q3["3 — Urgent but trivial behavior<br/>(looks like priority #1)"]
    end
    subgraph NotUrgent["NO URGENTE"]
        Q2["2 — Architecture<br/>(important, not urgent)"]
        Q4["4 — Noise<br/>(not important, ignore)"]
    end

    style Q1 fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style Q2 fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:3px
    style Q3 fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style Q4 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
```

Columna izquierda (`Q1`, `Q2`) = **importante**; columna derecha (`Q3`, `Q4`) = **no importante**. El bloque `URGENTE` agrupa los cuadrantes 1 y 3; el bloque `NO URGENTE` agrupa los cuadrantes 2 y 4.

- El **comportamiento** suele ser **urgente** pero no siempre importante.
- La **arquitectura** es **importante** pero casi nunca urgente.

El error clásico del negocio (y de muchos devs) es **elevar el cuadrante 3 (urgente pero no importante) por encima del cuadrante 2 (importante pero no urgente)**. Se atienden urgencias triviales y se abandona la arquitectura, que es lo que sostiene el proyecto a largo plazo.

## Cuál importa más

La respuesta contraintuitiva de Uncle Bob:

| Escenario | ¿Qué prefieres? |
|-----------|-----------------|
| Programa que **funciona** pero es **imposible de cambiar** | Servirá hasta que cambien los requisitos… y entonces dejará de servir. Al final: **inútil**. |
| Programa que **no funciona** pero es **fácil de cambiar** | Se puede hacer funcionar y mantenerlo funcionando. Al final: **útil**. |

> Un sistema que funciona pero no se puede cambiar quedará obsoleto en cuanto cambien los requisitos. Un sistema que se puede cambiar puede hacerse funcionar y mantenerse vivo.

Por eso, a largo plazo, **la estructura (arquitectura) tiene más valor que el comportamiento inmediato**. No porque el comportamiento no importe, sino porque la estructura es lo que permite seguir entregando comportamiento en el futuro.

## El deber del equipo de desarrollo

Los stakeholders no están capacitados para evaluar la arquitectura: solo ven el comportamiento. Por eso:

```mermaid
flowchart TD
    S["Stakeholders<br/>ask for urgent behavior"] ==> D{"Development<br/>team"}
    D ==> R["✅ Responsible for PROTECTING<br/>the system structure"]
    R ==> A["It's a fight, not a favor<br/>(see next document)"]

    style S fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style D fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:3px
    style R fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style A fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
```

Es **responsabilidad del equipo de desarrollo** defender la arquitectura, porque son los únicos que entienden su importancia. No es traición al negocio: es proteger el activo del negocio.

## Punto clave para recordar

> **El comportamiento es urgente; la estructura es importante.** Priorizar siempre lo urgente sobre lo importante mata al sistema a mediano plazo. Un software que no se puede cambiar termina siendo inútil.

---

Anterior: [← Caso de estudio: productividad](./03-caso-de-estudio-productividad.md) · Siguiente: [La lucha por la arquitectura →](./05-la-lucha-por-la-arquitectura.md)
