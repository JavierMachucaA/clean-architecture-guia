# 1. Testeabilidad y diseño orientado a pruebas

Los tests suelen tratarse como ciudadanos de segunda clase: código que se escribe rápido, que se acopla a cualquier detalle disponible y que nadie diseña con cuidado. Uncle Bob invierte esa idea por completo.

> Los tests son parte del sistema y, como tal, participan de la arquitectura. Deben diseñarse igual que cualquier otro componente.

## Los tests son usuarios del sistema

Desde el punto de vista de la arquitectura, **todos los tests son componentes**. Y como componentes, están sujetos a la **Regla de Dependencia**: apuntan hacia adentro, hacia el código que verifican, y nunca al revés.

- El sistema **no sabe** nada de los tests.
- Los tests **sí saben** del sistema.
- Por eso los tests son el ejemplo más externo de la arquitectura: el círculo más periférico de todos.

Esto tiene una consecuencia liberadora: como nada depende de los tests, los tests son el componente **más desacoplado** del sistema. Nunca deben ser desplegados y viven fuera del sistema en producción.

## El rol de la testeabilidad en el diseño

Un sistema difícil de testear casi siempre es un sistema mal diseñado. La dificultad para escribir un test es un **síntoma** de acoplamiento excesivo o de fronteras mal trazadas. Diseñar pensando en la prueba empuja hacia:

| Buen diseño (testeable) | Mal diseño (frágil) |
|-------------------------|---------------------|
| Reglas de negocio aisladas de la UI y la BD | Lógica mezclada con detalles de framework |
| Dependencias inyectadas y sustituibles | Dependencias creadas y ocultas dentro |
| Boundaries claros con interfaces | Llamadas directas a clases concretas |
| Comportamiento verificable sin arrancar todo el sistema | Necesita levantar toda la infraestructura |

## El patrón Humble Object aplicado a los tests

El **Humble Object** es un patrón de diseño para separar comportamientos difíciles de testear de comportamientos fáciles de testear. La idea: dividir un módulo en dos partes.

- La parte **humilde** (humble) contiene solo el código que es difícil de probar, reducido al mínimo, casi sin lógica.
- La parte **testeable** contiene toda la lógica interesante y se prueba sin depender de la parte difícil.

El caso clásico es la GUI: la presentación en pantalla es difícil de testear, así que se deja un *Presenter* con toda la lógica (testeable) y una *View* humilde que solo mueve datos a la pantalla.

```mermaid
flowchart LR
    subgraph Testeable["Zona testeable"]
        P["🧠 Presenter<br/>(toda la logica)"]
    end
    subgraph Humilde["Humble Object"]
        V["🖥️ View<br/>(solo pinta)"]
    end
    P ==>|View Model| V
    T["✅ Test"] -.verifica.-> P
    T -.NO verifica.-> V

    class P testeable
    class V view
    class T iface
    classDef testeable fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    classDef view fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    classDef iface fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
```

El mismo principio aplica a bases de datos (gateways humildes), servicios externos y cualquier frontera con el mundo real. La lógica se aleja del límite y se concentra donde sí se puede probar.

## Vista de capas: dónde viven los tests

```mermaid
flowchart TB
    T["✅ Tests<br/>(el circulo mas externo de todos)"]
    FW["🌐 Frameworks & Drivers<br/>(UI, DB, Web)"]
    IA["🔄 Interface Adapters"]
    UC["⚙️ Use Cases"]
    E["🟡 Entities"]

    T ==> FW ==> IA ==> UC ==> E

    class T iface
    class FW ext
    class IA adapter
    class UC nucleo
    class E entity
    classDef iface fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    classDef ext fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    classDef adapter fill:#6a1b9a,stroke:#ce93d8,color:#fff,stroke-width:2px
    classDef nucleo fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    classDef entity fill:#f9a825,stroke:#f57f17,color:#000,stroke-width:2px
```

Las dependencias SIEMPRE apuntan hacia adentro. Los tests envuelven al sistema, pero jamás forman parte de él en tiempo de ejecución.

## Punto clave para recordar

> Los tests son usuarios del sistema y siguen la Regla de Dependencia como cualquier otro componente. Diseñar para la testeabilidad —usando el patrón Humble Object para apartar lo difícil de probar— produce, casi como efecto secundario, una mejor arquitectura.

---

Siguiente: [02-fragil-arquitectura-de-pruebas.md](./02-fragil-arquitectura-de-pruebas.md)
