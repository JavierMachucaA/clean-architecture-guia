# Clean Architecture

> Documentación de referencia sobre los tópicos principales del libro **Clean Architecture: A Craftsman's Guide to Software Structure and Design** de Robert C. Martin (Uncle Bob).

## Descripción básica

**Clean Architecture** es un conjunto de principios y patrones para organizar el código de un sistema de forma que sea **independiente de frameworks, bases de datos, interfaces de usuario y detalles de infraestructura**. La idea central es que las reglas de negocio (lo que hace valioso al software) queden aisladas y protegidas de los detalles técnicos, que son volátiles y cambian con el tiempo.

El objetivo es lograr sistemas que sean:

- **Fáciles de mantener y evolucionar** a lo largo del tiempo.
- **Testeables** sin necesidad de UI, base de datos o servidores externos.
- **Independientes de la tecnología**, de modo que frameworks y herramientas puedan reemplazarse con bajo impacto.
- **Comprensibles**, con límites (boundaries) claros entre componentes.

La representación más conocida es el **diagrama de círculos concéntricos**: mientras más al centro, más abstracto y estable (reglas de negocio); mientras más afuera, más concreto y volátil (detalles). La **Regla de Dependencia** dicta que las dependencias del código fuente siempre apuntan hacia adentro, hacia las políticas de más alto nivel.

```
        +-----------------------------------------------+
        |   Frameworks & Drivers (Web, DB, UI, Devices) |
        |   +---------------------------------------+   |
        |   |   Interface Adapters                  |   |
        |   |   (Controllers, Gateways, Presenters) |   |
        |   |   +-------------------------------+   |   |
        |   |   |   Application Business Rules  |   |   |
        |   |   |   (Use Cases)                 |   |   |
        |   |   |   +-----------------------+   |   |   |
        |   |   |   | Enterprise Business   |   |   |   |
        |   |   |   | Rules (Entities)      |   |   |   |
        |   |   |   +-----------------------+   |   |   |
        |   |   +-------------------------------+   |   |
        |   +---------------------------------------+   |
        +-----------------------------------------------+

        Dirección de las dependencias  ---> hacia adentro
```

---

## Mapa de la guía

Cada tópico está desarrollado en su propia carpeta, con un README índice y varios documentos con diagramas (Mermaid + ASCII). Usa esta tabla para navegar:

| # | Tópico | Carpeta |
|---|--------|---------|
| 1 | Introducción y visión general | [01-introduccion-y-vision-general](./01-introduccion-y-vision-general/README.md) |
| 2 | Paradigmas de programación | [02-paradigmas-de-programacion](./02-paradigmas-de-programacion/README.md) |
| 3 | Principios de diseño: SOLID | [03-principios-solid](./03-principios-solid/README.md) |
| 4 | Principios de componentes | [04-principios-de-componentes](./04-principios-de-componentes/README.md) |
| 5 | Arquitectura: conceptos centrales | [05-arquitectura-conceptos-centrales](./05-arquitectura-conceptos-centrales/README.md) |
| 6 | Límites (Boundaries) | [06-limites-boundaries](./06-limites-boundaries/README.md) |
| 7 | Reglas de negocio | [07-reglas-de-negocio](./07-reglas-de-negocio/README.md) |
| 8 | La arquitectura limpia (círculos) | [08-la-arquitectura-limpia](./08-la-arquitectura-limpia/README.md) |
| 9 | Presenters, Humble Objects y adaptadores | [09-presenters-humble-objects](./09-presenters-humble-objects/README.md) |
| 10 | Límites parciales y niveles de abstracción | [10-limites-parciales](./10-limites-parciales/README.md) |
| 11 | Detalles como decisiones aplazables | [11-detalles-aplazables](./11-detalles-aplazables/README.md) |
| 12 | Organización del código y estructura de paquetes | [12-organizacion-del-codigo](./12-organizacion-del-codigo/README.md) |
| 13 | Temas transversales y complementarios | [13-temas-transversales](./13-temas-transversales/README.md) |

---

## Tópicos principales (detalle)

A continuación el desglose de los temas de cada tópico. El título de cada sección enlaza a su carpeta desarrollada.

### [1. Introducción y visión general](./01-introduccion-y-vision-general/README.md)
- Qué es diseño y qué es arquitectura (no son cosas distintas).
- El objetivo de la arquitectura: minimizar el costo de recursos humanos por comportamiento del sistema.
- El caso de estudio: cómo el desorden arquitectónico frena la productividad.
- El dilema de los desarrolladores: comportamiento vs. estructura.
- La lucha por la arquitectura frente a la presión por "sacar features".

### [2. A partir de los bloques: paradigmas de programación](./02-paradigmas-de-programacion/README.md)
- **Programación estructurada** — disciplina impuesta sobre la transferencia directa de control (eliminar el `goto`).
- **Programación orientada a objetos** — disciplina sobre la transferencia indirecta de control (polimorfismo) y la inversión de dependencias.
- **Programación funcional** — disciplina sobre la asignación de variables (inmutabilidad).
- Por qué cada paradigma *quita* capacidades más que agregarlas, y cómo eso impone orden.

### [3. Principios de diseño: SOLID](./03-principios-solid/README.md)
- **SRP** — Single Responsibility Principle (una razón para cambiar, ligado a actores).
- **OCP** — Open-Closed Principle (abierto a extensión, cerrado a modificación).
- **LSP** — Liskov Substitution Principle (sustitución de subtipos).
- **ISP** — Interface Segregation Principle (no depender de lo que no se usa).
- **DIP** — Dependency Inversion Principle (depender de abstracciones, no de concreciones).

### [4. Principios de componentes](./04-principios-de-componentes/README.md)
- **Cohesión de componentes:**
  - REP — Reuse/Release Equivalence Principle.
  - CCP — Common Closure Principle.
  - CRP — Common Reuse Principle.
- **Acoplamiento de componentes:**
  - ADP — Acyclic Dependencies Principle (evitar ciclos de dependencias).
  - SDP — Stable Dependencies Principle (depender en dirección de la estabilidad).
  - SAP — Stable Abstractions Principle (estabilidad proporcional a abstracción).
- Diagramas de tensión y el "main sequence".

### [5. Arquitectura: conceptos centrales](./05-arquitectura-conceptos-centrales/README.md)
- Qué es la arquitectura de un sistema y qué persigue.
- **Mantener abiertas las opciones** (keeping options open) y diferir decisiones.
- Independencia: casos de uso, operación, desarrollo y despliegue.
- **Desacoplamiento por capas** y por casos de uso.
- Duplicación real vs. duplicación accidental.
- Modos de desacoplamiento: a nivel de código (source), de despliegue (deployment) y de servicios.

### [6. Límites (Boundaries)](./06-limites-boundaries/README.md)
- Qué son los límites y por qué trazarlos.
- **Boundary anticipation**: cuándo y dónde dibujar líneas.
- Componentes plugin y la arquitectura plugin.
- Anatomía de un límite (boundary crossing).
- El **flujo de control** vs. la **dirección de las dependencias** (por qué a veces son opuestos).

### [7. Reglas de negocio](./07-reglas-de-negocio/README.md)
- **Entities** — reglas de negocio críticas de la empresa (Enterprise Business Rules).
- **Use Cases** — reglas de negocio de la aplicación (Application Business Rules).
- Modelos de request y response.
- Cómo las entidades no conocen los casos de uso, pero los casos de uso conocen las entidades.

### [8. La arquitectura limpia (el modelo de círculos)](./08-la-arquitectura-limpia/README.md)
- Los cuatro anillos: Entities, Use Cases, Interface Adapters, Frameworks & Drivers.
- **La Regla de Dependencia** (Dependency Rule).
- Cruce de límites y la **inversión de dependencias** en las fronteras.
- Qué datos cruzan las fronteras (DTOs simples, no entidades).

### [9. Presenters, Humble Objects y adaptadores](./09-presenters-humble-objects/README.md)
- **Humble Object Pattern** para separar lo testeable de lo difícil de testear.
- **Presenters** y **View Models**.
- Gateways de base de datos y de servicios.
- Mappers entre capas.

### [10. Límites parciales y niveles de abstracción](./10-limites-parciales/README.md)
- Full boundaries vs. **partial boundaries** (costo/beneficio).
- Estrategias: "skip the last step", one-dimensional boundaries, facades.
- Cómo elegir el nivel de rigor según el proyecto.

### [11. Detalles como decisiones aplazables](./11-detalles-aplazables/README.md)
- La **base de datos es un detalle** (no el corazón del sistema).
- La **web es un detalle** (una UI más entre muchas).
- Los **frameworks son detalles** (evitar el matrimonio con el framework).
- Datos vs. objetos y por qué las bases de datos relacionales son un detalle.

### [12. Organización del código y estructura de paquetes](./12-organizacion-del-codigo/README.md)
- **Screaming Architecture**: la estructura debe "gritar" el propósito del sistema, no el framework.
- Empaquetado por capas vs. por feature vs. por componente.
- Ports & Adapters (Arquitectura Hexagonal) y su relación con Clean Architecture.
- Main como el "sucio" componente de más bajo nivel que ensambla todo.

### [13. Temas transversales y complementarios](./13-temas-transversales/README.md)
- Testeabilidad y el diseño orientado a pruebas.
- La **frágil arquitectura de pruebas** y el patrón de API de tests.
- Servicios: ¿son realmente arquitectura? (el mito de los microservicios que desacoplan por sí solos).
- El componente **Main** y la inyección de dependencias.
- Arquitectura embebida (clean embedded architecture).

---

## Cómo usar esta guía

1. Empieza por la **descripción básica** y el diagrama para tener el modelo mental.
2. Profundiza los **principios (SOLID y de componentes)** antes de los patrones arquitectónicos.
3. Practica trazando **boundaries** en un proyecto real y aplicando la **Regla de Dependencia**.
4. Trata siempre base de datos, UI y frameworks como **detalles reemplazables**.

## Referencia

- Robert C. Martin, *Clean Architecture: A Craftsman's Guide to Software Structure and Design*, Prentice Hall, 2017.
