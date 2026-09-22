# 4. Anatomía de un boundary crossing y el flujo de control

## Qué es un boundary crossing

> Un **cruce de límite** (boundary crossing) es una llamada de una función a otra que está al otro lado de la línea, pasando datos a través de ella.

En tiempo de ejecución, cruzar un límite es simplemente una llamada de función. Lo interesante no es la llamada en sí, sino cómo se gestionan **la dirección de la dependencia del código fuente** y **la dirección del flujo de control**, que no siempre coinciden.

La anatomía típica de un cruce bien diseñado tiene tres piezas:

| Pieza | Papel |
|-------|-------|
| **Llamador de alto nivel** | Contiene la política / regla de negocio; inicia la acción |
| **Interfaz (frontera)** | Abstracción que define el contrato del cruce |
| **Implementación de bajo nivel** | El detalle (DB, UI, dispositivo) que hace el trabajo concreto |

## Flujo de control vs. dirección de las dependencias

El flujo de control es **quién llama a quién en ejecución**. La dependencia del código es **quién menciona/importa a quién en el fuente**. La clave arquitectónica de Uncle Bob:

> A veces el flujo de control cruza el límite en **un sentido** mientras la dependencia del código apunta en el sentido **contrario**. Eso se logra con la **inversión de dependencias (DIP)**.

### Cuando coinciden (sin inversión)

```mermaid
flowchart LR
    AN["Alto nivel<br/>(política)"] -->|"llama Y depende"| BN["Bajo nivel<br/>(detalle: DB / UI)"]

    style AN fill:#2e7d32,stroke:#1b5e20,stroke-width:2px,color:#fff
    style BN fill:#c62828,stroke:#8e0000,stroke-width:2px,color:#fff
```

Aquí el flujo de control **y** la dependencia del código van en el mismo sentido: el alto nivel depende del detalle. Cambiar el detalle obliga a tocar la política. Es justo lo que queremos evitar en un límite.

### Cuando se oponen (con inversión de dependencias)

Insertamos una interfaz que **el alto nivel posee** y que **el bajo nivel implementa**. Ahora el flujo de control sigue yendo del alto al bajo nivel, pero la dependencia del código apunta del bajo nivel hacia la abstracción del alto nivel.

```mermaid
flowchart LR
    AN["Alto nivel<br/>(política)"] -->|usa| I["«interface»<br/>frontera del límite"]
    BN["Bajo nivel<br/>(detalle: DB / UI)"] -.implementa.-> I

    style AN fill:#2e7d32,stroke:#1b5e20,stroke-width:2px,color:#fff
    style BN fill:#c62828,stroke:#8e0000,stroke-width:2px,color:#fff
    style I fill:#1565c0,stroke:#0d47a1,stroke-width:2px,color:#fff,stroke-dasharray: 5 5
```

El punto fino es que **el flujo de control y la dependencia del código apuntan en sentidos opuestos**. El siguiente diagrama los muestra a la vez sobre el mismo límite: la flecha sólida (control) cruza hacia el detalle, mientras la flecha punteada (dependencia) apunta de vuelta hacia la abstracción del núcleo.

```mermaid
flowchart LR
    subgraph nucleo["NÚCLEO (política)"]
        AN["Alto nivel"]
        I["«interface»<br/>posee el núcleo"]
    end
    subgraph detalle["DETALLE (plugin)"]
        BN["Bajo nivel<br/>(DB / UI)"]
    end

    AN -->|"1 · FLUJO DE CONTROL (ejecución)"| BN
    BN -.->|"2 · DEPENDENCIA DEL CÓDIGO (implementa)"| I

    style AN fill:#2e7d32,stroke:#1b5e20,stroke-width:2px,color:#fff
    style I fill:#1565c0,stroke:#0d47a1,stroke-width:2px,color:#fff,stroke-dasharray: 5 5
    style BN fill:#c62828,stroke:#8e0000,stroke-width:2px,color:#fff
    style nucleo fill:#1b5e20,stroke:#2e7d32,stroke-width:2px,color:#fff
    style detalle fill:#8e0000,stroke:#c62828,stroke-width:2px,color:#fff
```

En ejecución el control sale del núcleo hacia el detalle (flecha 1), pero en el código el detalle es quien depende del núcleo al implementar su interfaz (flecha 2). Esa oposición deliberada es la inversión de dependencias (DIP).

## Por qué esto importa

Poder invertir la dependencia respecto al flujo de control es lo que hace posible todo lo demás del tópico:

- El **negocio no depende del detalle** aunque en ejecución lo invoque.
- El detalle (DB, UI) queda como **plugin** que apunta hacia el núcleo.
- El límite se vuelve una **línea de desacoplamiento real**, no solo una separación de carpetas.

```mermaid
flowchart TD
    A["Flujo de control<br/>necesita ir hacia el detalle"] --> B["Insertar interfaz<br/>en la frontera"]
    B --> C["El detalle implementa<br/>la interfaz del núcleo"]
    C --> D["Dependencia del código<br/>apunta hacia el núcleo"]
    D --> E["Límite desacoplado +<br/>arquitectura plugin"]

    style E fill:#2e7d32,stroke:#1b5e20,stroke-width:2px,color:#fff
```

## Punto clave para recordar

> **El flujo de control y la dirección de las dependencias no tienen por qué coincidir.** Con la inversión de dependencias, el control puede cruzar el límite hacia el detalle mientras la dependencia del código apunta de vuelta al negocio. Esa oposición deliberada es lo que convierte un límite en un desacoplamiento verdadero.

---

Anterior: [← Arquitectura plugin](./03-arquitectura-plugin.md) · Volver al índice: [README del tópico](./README.md)
