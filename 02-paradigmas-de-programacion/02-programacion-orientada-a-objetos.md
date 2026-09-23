# 2. Programación orientada a objetos

## Qué restringe

> **La OO impone disciplina sobre la transferencia indirecta de control** (los punteros a función).

El polimorfismo, bajo el capó, se implementa con punteros a función. La OO nos da una forma **segura y disciplinada** de usar ese mecanismo, en lugar de manipular punteros a mano (peligroso y propenso a errores).

## Qué NO es la OO (según Uncle Bob)

Martin descarta las definiciones populares:

| Definición popular | Por qué no basta |
|--------------------|------------------|
| "Encapsulación" | C ya encapsulaba con `.h`/`.c`; la OO incluso la debilita. |
| "Herencia" | Se podía simular en C antes de la OO. |
| "Es modelar el mundo real" | Vago y no distingue a la OO de nada. |

Lo que **sí** define a la OO de forma distintiva es el **polimorfismo seguro**.

## El verdadero poder: inversión de dependencias

El polimorfismo permite invertir la dirección de una dependencia respecto al flujo de control. Este es el aporte arquitectónico decisivo.

### Sin inversión (flujo y dependencia van juntos)

```mermaid
flowchart LR
    HL["🧠 High-Level Module<br/>(policy)"] ==>|depende de| LL["⚙️ Low-Level Module<br/>(detalle: e.g. driver)"]

    style HL fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:3px
    style LL fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
```

El alto nivel queda atado al detalle. Cambiar el detalle obliga a recompilar y redeployar el alto nivel.

### Con inversión (una interfaz invierte la flecha)

```mermaid
flowchart LR
    HL["🧠 High-Level Module<br/>(policy)"] ==>|usa| I["🔌 «interface»<br/>Abstraction"]
    LL["⚙️ Low-Level Module<br/>(detalle)"] -.implementa.-> I

    style HL fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:3px
    style I fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px,stroke-dasharray: 5 5
    style LL fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
```

Ahora el detalle **depende** de la abstracción, y el flujo de control (alto → bajo) va en dirección **opuesta** a la dependencia de código (bajo → abstracción ← alto).

```mermaid
flowchart LR
    subgraph Flow["🔄 Flujo de control"]
        direction LR
        H1["🧠 High-Level"] ==> L1["⚙️ Low-Level"]
    end
    subgraph Dep["📦 Dependencia de código (invertida)"]
        direction LR
        H2["🧠 High-Level"] ==> I2["🔌 Interface"]
        L2["⚙️ Low-Level"] ==> I2
    end

    style H1 fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style L1 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style H2 fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style L2 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style I2 fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
```

El flujo de control va de alto nivel hacia bajo nivel, mientras que la dependencia de código queda **invertida**: tanto el alto como el bajo nivel apuntan hacia la interfaz.

## Por qué esto es la clave de la arquitectura

Con inversión de dependencias puedes decidir **qué depende de qué**, sin importar quién llama a quién. Eso permite:

```mermaid
flowchart TD
    A["🔄 Poder invertir cualquier dependencia"] ==> B["🗄️ Los detalles (DB, UI, framework)<br/>dependen de las reglas de negocio"]
    B ==> C["🧠 Las reglas de negocio NO dependen<br/>de ningún detalle"]
    C ==> D["✅ Boundaries + arquitectura plugin<br/>(base de Clean Architecture)"]

    style A fill:#6a1b9a,stroke:#ce93d8,color:#fff,stroke-width:2px
    style B fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style C fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:3px
    style D fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
```

Esto habilita el **despliegue y desarrollo independiente**: puedes compilar y desplegar los componentes de bajo nivel por separado de las políticas de negocio.

## Punto clave para recordar

> **La OO es polimorfismo seguro, y su regalo a la arquitectura es la inversión de dependencias.** Gracias a ella, los detalles pueden depender de las reglas de negocio y no al revés.

---

Anterior: [← Programación estructurada](./01-programacion-estructurada.md) · Siguiente: [Programación funcional →](./03-programacion-funcional.md)
