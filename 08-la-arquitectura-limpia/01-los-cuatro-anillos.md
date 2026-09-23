# 1. Los cuatro anillos

## De dentro hacia afuera

La Arquitectura Limpia organiza el sistema en **anillos concéntricos**. Cuanto más hacia el **centro**, más **abstracto y estable** es el código (reglas de negocio de alto nivel). Cuanto más hacia el **borde**, más **concreto y volátil** (mecanismos, detalles, frameworks).

> El número de anillos no es sagrado: pueden ser más de cuatro. Lo que nunca cambia es que las dependencias siempre apuntan hacia adentro.

### Los anillos, de fuera hacia dentro

```mermaid
flowchart TB
    FD["🔵 Frameworks & Drivers<br/>Web · DB · UI · Devices<br/>· borde · muy volátil ·"]
    IA["🟣 Interface Adapters<br/>Controllers · Presenters · Gateways<br/>· volatilidad media ·"]
    UC["🔴 Use Cases<br/>Reglas de negocio de la aplicación<br/>· baja volatilidad ·"]
    EN["🟡 Entities<br/>Reglas de negocio de la empresa (críticas)<br/>· núcleo · muy estable ·"]

    FD --> IA --> UC --> EN

    style FD fill:#455a64,stroke:#263238,stroke-width:2px,color:#fff
    style IA fill:#6a1b9a,stroke:#4a148c,stroke-width:2px,color:#fff
    style UC fill:#c62828,stroke:#8e0000,stroke-width:2px,color:#fff
    style EN fill:#f9a825,stroke:#f57f17,stroke-width:2px,color:#000
```

Cada caja apunta a la de adentro: esa flecha es la **dirección de la dependencia**. Va siempre hacia el núcleo (`Frameworks → Interface Adapters → Use Cases → Entities`), nunca al revés. De arriba hacia abajo el código se vuelve **más abstracto y estable**; de abajo hacia arriba, **más concreto y volátil**. Las Entities, en el centro, no dependen de ningún otro anillo.

## Qué vive en cada anillo

| Anillo | Nombre | Contenido | Volatilidad |
|--------|--------|-----------|-------------|
| Centro | **Entities** | Reglas de negocio de la empresa: objetos con los datos y métodos más generales y de más alto nivel. Las más estables. | Muy baja |
| 2.º | **Use Cases** | Reglas de negocio específicas de la **aplicación**: orquestan el flujo de datos hacia y desde las Entities. | Baja |
| 3.º | **Interface Adapters** | Convierten datos entre el formato cómodo para los casos de uso/entidades y el formato cómodo para agentes externos (Controllers, Presenters, Gateways). | Media |
| Borde | **Frameworks & Drivers** | Detalles: la web, la base de datos, la UI, dispositivos, herramientas. Aquí va "el pegamento" hacia afuera. | Muy alta |

## Detalle por anillo

- **Entities** — encapsulan las reglas más generales del negocio. Podrían existir aunque no hubiera ninguna aplicación concreta. No cambian por decisiones operativas de una app.
- **Use Cases** — contienen las reglas específicas de la aplicación e implementan todos los casos de uso del sistema. Dirigen el baile de las Entities pero no deberían verse afectados por cambios de UI o base de datos.
- **Interface Adapters** — el conjunto de adaptadores que traducen datos. El MVC de una GUI vive aquí; también los repositorios que hablan con la base de datos.
- **Frameworks & Drivers** — el anillo más externo, hecho de herramientas y detalles. Normalmente escribes poco código aquí, salvo el que conecta hacia adentro.

## Punto clave para recordar

> **Cuatro anillos, un solo orden: del núcleo estable (Entities) al borde volátil (Frameworks).** Cuanto más adentro, más abstracto y más protegido del cambio.

---

Siguiente: [La Regla de Dependencia →](./02-la-regla-de-dependencia.md)
