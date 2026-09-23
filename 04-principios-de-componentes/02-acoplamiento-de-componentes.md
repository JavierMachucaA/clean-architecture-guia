# 2. Acoplamiento de componentes

Si la cohesión decide *qué clases van juntas*, el acoplamiento decide *cómo se conectan los componentes entre sí*. Robert C. Martin propone tres principios que gobiernan el grafo de dependencias entre componentes:

- **ADP** — Acyclic Dependencies Principle (Principio de Dependencias Acíclicas).
- **SDP** — Stable Dependencies Principle (Principio de Dependencias Estables).
- **SAP** — Stable Abstractions Principle (Principio de Abstracciones Estables).

## ADP — Dependencias acíclicas

> *No permitas ciclos en el grafo de dependencias de componentes.*

El grafo de dependencias entre componentes debe ser un **DAG** (grafo dirigido acíclico). Un ciclo hace que dos o más componentes queden atados: no puedes compilar, probar ni liberar uno sin arrastrar a los demás. El clásico "síndrome de la mañana siguiente": alguien tocó un componente del ciclo y ahora nada compila.

Considera este grafo con un ciclo entre `Entities`, `Authorization` e `Interactors`:

```mermaid
flowchart TD
    Main["Main"] ==> Interactors["Interactors"]
    Interactors ==> Entities["Entities"]
    Entities ==> Authorization["Authorization"]
    Authorization ==> Interactors
    Interactors -.->|"⛔ CICLO"| Entities

    style Main fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style Interactors fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style Entities fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style Authorization fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
```

`Authorization → Interactors → Entities → Authorization` forma un ciclo. Ninguno de esos tres se puede liberar de forma independiente.

### Cómo romper un ciclo

Hay **dos técnicas** para eliminar un ciclo:

1. **Aplicar DIP (Inversión de Dependencias):** creas una interfaz en el componente que *quiere* invertir la dependencia. La clase que antes se llamaba directamente pasa a implementar esa interfaz. La flecha de dependencia se invierte.

2. **Crear un componente nuevo:** extraes las clases de las que ambos dependen a un componente adicional. Todos apuntan hacia el componente nuevo y el ciclo desaparece.

```mermaid
flowchart TD
    subgraph SolucionDIP["🔄 Solución con DIP"]
        A1["Authorization"] ==> I1["Interface in Authorization"]
        E1["Entities"] -.->|implementa| I1
        In1["Interactors"] ==> E1
        In1 ==> A1
    end

    style A1 fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style I1 fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:3px
    style E1 fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style In1 fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
```

Comparación de las dos técnicas. En **DIP** se introduce la interfaz `IB` y la flecha `B → IB` queda invertida respecto al ciclo original; en **Componente nuevo** todos apuntan hacia `C`, de modo que el ciclo desaparece:

```mermaid
flowchart LR
    subgraph Antes["⛔ ANTES (ciclo)"]
        direction TB
        A0["A"] ==> B0["B"]
        B0 ==> C0["C"]
        C0 ==> A0
    end
    subgraph DIP["✅ DIP"]
        direction TB
        A1["A"] ==> IB1["IB"]
        B1["B"] -.->|implementa| IB1
        A1 ==> B1
    end
    subgraph Nuevo["✅ COMPONENTE NUEVO"]
        direction TB
        A2["A"] ==> C2["C"]
        B2["B"] ==> C2
    end

    style A0 fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style B0 fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style C0 fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style A1 fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style B1 fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style IB1 fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:3px
    style A2 fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style B2 fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style C2 fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:3px
```

## SDP — Dependencias estables

> *Depende en la dirección de la estabilidad.* Un componente solo debe depender de componentes **más estables** que él.

La **estabilidad** no mide con qué frecuencia cambia un componente, sino cuánto **esfuerzo** cuesta cambiarlo. Un componente del que dependen muchos otros es difícil de cambiar (hay que coordinar a todos sus dependientes): es *estable*. Un componente que no depende de nadie y del que nadie depende es *inestable*: se cambia sin consecuencias.

Se cuantifica con la **métrica de inestabilidad I**:

```
        Fan-out
I = -----------------
    Fan-in + Fan-out
```

- **Fan-in (dependencias entrantes):** número de clases fuera del componente que dependen de clases dentro de él.
- **Fan-out (dependencias salientes):** número de clases dentro del componente que dependen de clases fuera de él.
- **I = 0** → máxima estabilidad (muchos dependen de él, él no depende de nadie).
- **I = 1** → máxima inestabilidad (no depende nadie de él, él depende de todos).

El SDP exige que **la I disminuya en la dirección de las flechas**: cada dependencia debe apuntar hacia un componente con I menor o igual.

```mermaid
flowchart LR
    subgraph Correcto["✅ Correcto: I decrece siguiendo las flechas"]
        direction LR
        Cx["Cx<br/>I=1.0 · inestable"] ==> Cy["Cy<br/>I=0.5 · intermedio"]
        Cy ==> Cz["Cz<br/>I=0.0 · estable"]
    end
    subgraph Violacion["⛔ Violación: un componente estable depende de uno inestable"]
        direction LR
        Ca["Ca<br/>I=0.0 · estable"] ==> Cb["Cb<br/>I=1.0 · inestable"]
    end

    style Cx fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
    style Cy fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    style Cz fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style Ca fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style Cb fill:#c62828,stroke:#ff8a80,color:#fff,stroke-width:2px
```

## SAP — Abstracciones estables

> *Un componente debe ser tan abstracto como estable.*

Aquí surge una pregunta: si los componentes estables son difíciles de cambiar, ¿cómo evitamos que esa rigidez impida evolucionar el sistema? La respuesta es hacerlos **abstractos**. Un componente estable pero **abstracto** (lleno de interfaces y clases abstractas) puede extenderse sin modificarse, cumpliendo el OCP. Así, la estabilidad no impide la flexibilidad.

Se cuantifica con la **métrica de abstracción A**:

```
    Número de clases abstractas e interfaces
A = ----------------------------------------
              Número total de clases
```

- **A = 0** → componente totalmente concreto.
- **A = 1** → componente totalmente abstracto (solo interfaces y clases abstractas).

El SAP conecta con el SDP: **estabilidad y abstracción deben ir de la mano.** Un componente estable (I≈0) debería ser abstracto (A≈1); un componente inestable (I≈1) debería ser concreto (A≈0). Cuando esta proporción se rompe, el componente cae en zonas problemáticas que se estudian en el siguiente documento.

| Principio | Regla | Métrica | Valor ideal |
|-----------|-------|---------|-------------|
| ADP | Sin ciclos en el grafo | — | DAG (0 ciclos) |
| SDP | Depender hacia lo estable | I = Fan-out / (Fan-in + Fan-out) | I decrece siguiendo flechas |
| SAP | Tan abstracto como estable | A = abstractas / totales | A ≈ 1 − I |

## Punto clave para recordar

> El grafo de componentes debe ser **acíclico** (ADP), sus dependencias deben apuntar **hacia lo estable** (SDP: I = Fan-out / (Fan-in + Fan-out)), y lo estable debe ser **abstracto** (SAP: A = abstractas / totales). Juntos garantizan que los detalles volátiles dependan de políticas estables y abstractas, y nunca al revés.

---

Anterior: [1. Cohesión de componentes](./01-cohesion-de-componentes.md) · Siguiente: [3. Main Sequence y tensión](./03-main-sequence-y-tension.md)
