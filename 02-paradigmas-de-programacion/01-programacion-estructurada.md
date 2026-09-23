# 1. Programación estructurada

## Qué restringe

> **La programación estructurada impone disciplina sobre la transferencia directa de control.**

En términos prácticos: **elimina el `goto`** y lo reemplaza por tres estructuras de control disciplinadas.

## El descubrimiento de Dijkstra

Edsger Dijkstra demostró que cualquier programa puede construirse con solo **tres estructuras**, sin saltos arbitrarios:

```mermaid
flowchart TD
    A["⚙️ Sequence<br/>(un paso tras otro)"]
    B["⚙️ Selection<br/>(if / else, switch)"]
    C["⚙️ Iteration<br/>(while, for)"]
    A --- B --- C

    style A fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style B fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style C fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
```

El `goto` permitía saltar a cualquier parte del código, creando "código spaghetti" imposible de razonar. Al prohibirlo, cada bloque tiene una entrada y una salida claras.

```mermaid
flowchart LR
    subgraph Chaos["⛔ Con goto (caos)"]
        direction TB
        G1["Block 1"] ==> G2["Block 2"]
        G2 ==> G3["Block 3"]
        G3 ==> G1
        G2 ==> G1
    end
    subgraph Ordered["✅ Estructurada (orden)"]
        direction TB
        S1["🖥️ Entry"] ==> S2["⚙️ Block"]
        S2 ==> S3["📦 Exit"]
    end

    style G1 fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style G2 fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style G3 fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style S1 fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    style S2 fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style S3 fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
```

A la izquierda, los saltos cruzados del `goto` entran y salen por cualquier punto (caos). A la derecha, el bloque estructurado tiene **una entrada y una salida** claras.

## Por qué importa: descomposición y prueba

La disciplina estructurada permite **descomponer** un problema grande en funciones más pequeñas, cada una razonable por separado:

```mermaid
flowchart TD
    P["🧠 Big Problem"] ==> F1["⚙️ Function A"]
    P ==> F2["⚙️ Function B"]
    P ==> F3["⚙️ Function C"]
    F2 ==> F2a["⚙️ Subfunction B1"]
    F2 ==> F2b["⚙️ Subfunction B2"]

    style P fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:3px
    style F1 fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    style F2 fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    style F3 fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    style F2a fill:#6a1b9a,stroke:#ce93d8,color:#fff,stroke-width:2px
    style F2b fill:#6a1b9a,stroke:#ce93d8,color:#fff,stroke-width:2px
```

## La conexión con la ciencia y las pruebas

Uncle Bob subraya un punto profundo: **no se puede probar que un programa es correcto, solo se puede probar que es incorrecto** (falsación, como en la ciencia).

| Idea | Explicación |
|------|-------------|
| No hay prueba matemática de corrección | Como en física, las pruebas no "demuestran" corrección; buscan fallos. |
| Los tests muestran presencia de bugs | Un test que pasa no garantiza ausencia de errores, solo que no encontró ese. |
| La estructura hace testeable el código | Bloques con entrada/salida claras son unidades falsables (probables por tests). |

> La programación estructurada nos da unidades de código que pueden **falsarse** mediante pruebas. Esa testeabilidad es la base para confiar en el software.

## Relevancia arquitectónica

La descomposición funcional disciplinada es lo que permite dividir un sistema en módulos y componentes comprobables. Sin ella, no habría forma de razonar sobre las piezas de una arquitectura.

## Punto clave para recordar

> **La programación estructurada quita el `goto` y a cambio da bloques comprobables.** Es la base para razonar, descomponer y testear el software.

---

Siguiente: [Programación orientada a objetos →](./02-programacion-orientada-a-objetos.md)
