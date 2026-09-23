# 2. Mantener las opciones abiertas

## Software = *soft* + *ware*

La palabra *software* es un compuesto: *soft* (blando) y *ware* (producto). El software se inventó para ser **blando**, es decir, fácil de cambiar. Si la máquina no puede cambiar de comportamiento con facilidad, la habríamos llamado *hardware*.

De ahí nace la misión de la arquitectura:

> Una buena arquitectura hace que el sistema sea **fácil de cambiar**, dejando **tantas opciones abiertas como sea posible, durante el mayor tiempo posible**.

```mermaid
flowchart LR
    S["📦 Software = soft<br/>(made to change)"] ==> A["🧠 Architecture"]
    A ==> O["✅ Keeps options open<br/>as long as possible"]
    O ==> D["⚙️ Deferred decisions =<br/>less risk, more information"]

    style S fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    style A fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:3px
    style O fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style D fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
```

## Política vs detalles

Todo sistema de software se puede descomponer en dos elementos: **política** y **detalles**.

- La **política** encarna las reglas y procedimientos del negocio. Es el verdadero valor del sistema.
- Los **detalles** son lo que hace falta para que humanos y otros sistemas se comuniquen con esa política, pero **no impactan** en el comportamiento de la política.

Ejemplos típicos de detalles: la base de datos, el framework web, el servidor, los protocolos, el formato de entrada/salida. Son **decisiones aplazables**.

| Elemento | Qué es | ¿Impacta la regla de negocio? | ¿Se puede diferir? |
|----------|--------|-------------------------------|--------------------|
| Política | Reglas y casos de uso del negocio | Es la regla misma | No, es el núcleo |
| Base de datos | Dónde se guardan los datos | No | Sí |
| Framework web | Cómo se entrega por HTTP | No | Sí |
| Servidor / despliegue | Dónde y cómo corre | No | Sí |

El buen arquitecto **maximiza la cantidad de decisiones no tomadas**.

```mermaid
flowchart TB
    CORE["🧠 Business rules + use cases<br/>(policy, stable)<br/>NON-deferrable decision"]
    CORE ==> DB["🗄️ DB<br/>which engine?"]
    CORE ==> WEB["🖥️ Web<br/>REST or GraphQL?"]
    CORE ==> FW["⚙️ Framework<br/>which one?"]
    CORE ==> SRV["📦 Server<br/>on-prem or cloud?"]

    style CORE fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:3px
    style DB fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style WEB fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style FW fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style SRV fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
```

El núcleo (política, estable) es una decisión **no** aplazable; la periferia (DB, web, framework, servidor) son detalles **aplazables**. La política no depende de los detalles: por eso las flechas apuntan hacia ellos y no al revés.

## Diferir decisiones da poder

Aplazar una decisión sobre un detalle tiene dos beneficios concretos:

1. **Más información al decidir.** Cuanto más tarde eliges la base de datos, más sabes sobre cómo se accede realmente a los datos.
2. **Más experimentos posibles.** Si la política no depende del detalle, puedes probar varias opciones (por ejemplo, distintas bases de datos) sin reescribir el negocio.

```mermaid
flowchart TD
    P["🧠 Business policy<br/>(does not know which DB it uses)"] ==>|talks against| I["📦 Interface / boundary"]
    I ==> DB1["🗄️ Option A:<br/>PostgreSQL"]
    I ==> DB2["🗄️ Option B:<br/>MongoDB"]
    I ==> DB3["🗄️ Option C:<br/>in-memory files<br/>(for test)"]

    style P fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:3px
    style I fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    style DB1 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style DB2 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style DB3 fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
```

Si la decisión de base de datos ya está tomada y clavada en el corazón del sistema, cambiarla cuesta una fortuna. Si está diferida detrás de un boundary, es solo un detalle intercambiable.

## El objetivo del arquitecto

El arquitecto quiere una estructura que:

- reconozca la política como el elemento más esencial del sistema, y
- vuelva los detalles **irrelevantes** para esa política.

Así las decisiones sobre detalles pueden retrasarse o incluso cambiarse mucho después, cuando ya no cuesta caro.

## Punto clave para recordar

> El buen arquitecto **maximiza las decisiones no tomadas**. Separa la política (el negocio, estable y valioso) de los detalles (DB, web, framework: aplazables), de modo que el sistema conserve la mayor cantidad de opciones abiertas el mayor tiempo posible.

---

Anterior: [← Qué es la arquitectura](./01-que-es-la-arquitectura.md) · Siguiente: [Independencia →](./03-independencia.md)
