# 4. Mappers entre capas

## Por qué los datos cambian de forma al cruzar un límite

Cada capa de Clean Architecture tiene su **propia representación de los datos**, adaptada a lo que esa capa necesita. Cuando la información cruza un límite, se **traduce** de una forma a otra mediante un **Mapper**.

> Es una violación de la regla de dependencia pasar una fila de base de datos hacia adentro. No queremos que las capas internas conozcan nada de las externas.

Si dejáramos que la misma estructura viajara por todas las capas, la forma del detalle (una fila de BD, un JSON de la API) se filtraría hacia el núcleo y ataría las reglas de negocio al framework.

## Una estructura por capa

```mermaid
flowchart LR
    R["🗄️ OrderRecord<br/>(columnas SQL)"] ==>|Mapper| E["🧠 Order (entidad)<br/>(reglas de negocio)"]
    E ==>|Mapper| VM["📦 OrderViewModel<br/>(textos listos)"]

    style R fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style E fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style VM fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
```

Cada frontera tiene un mapper que convierte de la estructura de una capa a la de la otra. Así los datos entran "traducidos" al idioma de cada capa.

## Qué hace y qué no hace un Mapper

```mermaid
flowchart LR
    R["🗄️ OrderRecord<br/>(capa BD)"] ==>|"🔄 Mapper DB→domain"| E["🧠 Order<br/>(entidad)"]
    E ==>|"🔄 Mapper domain→VM"| VM["📦 OrderViewModel<br/>(capa UI)"]
    VM -.->|"⛔ nunca al revés atraviesa"| E

    style R fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style E fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style VM fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
```

El Mapper copia y transforma campos; **no** contiene reglas de negocio. Es un traductor, no un lugar para esconder lógica.

## La dependencia siempre apunta hacia adentro

```mermaid
flowchart TD
    subgraph Externa["Detalle (BD / UI)"]
        REC["📦 Record / DTO / ViewModel"]
        M["🔄 Mapper"]
    end
    subgraph Interna["Dominio / casos de uso"]
        ENT["🧠 Entity"]
    end
    M ==>|conoce| REC
    M ==>|conoce| ENT
    ENT -.->|"⛔ no conoce"| REC

    style REC fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    style M fill:#6a1b9a,stroke:#ce93d8,color:#fff,stroke-width:2px
    style ENT fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
```

El Mapper vive en la capa externa y conoce **ambas** formas. La entidad interna, en cambio, **no conoce** la estructura externa. Por eso el mapper puede tocar los dos lados sin romper la regla de dependencia.

## Comparación de estructuras

| Capa | Estructura | Contiene | Ejemplo de campo |
|------|-----------|----------|------------------|
| Base de datos | Record / Row | Columnas tal cual | `estado_id = 2` |
| Casos de uso / dominio | Entidad | Reglas y tipos ricos | `estado = PAGADO` |
| Interfaz | View Model | Textos ya formateados | `estadoTexto = "Pagado"` |

Un mismo pedido se ve distinto en cada capa; el mapper hace las conversiones entre esas vistas.

## Ejemplo

❌ La fila de la BD viaja hasta el dominio y lo contamina:

```java
class CalculateShipping {
    double calculate(ResultSet row) throws SQLException { // depends on JDBC
        return row.getDouble("weight") * RATE;            // detail in the core
    }
}
```

✅ Un mapper traduce a entidad; el dominio ignora la BD:

```java
// Mapper in the outer layer
class OrderMapper {
    Order toDomain(ResultSet row) throws SQLException {
        return new Order(
            row.getString("id"),
            row.getDouble("weight"),
            Status.from(row.getInt("status_id"))
        );
    }
}

// Domain: clean, testable, no JDBC
class CalculateShipping {
    double calculate(Order order) {
        return order.getWeight() * RATE;
    }
}
```

El detalle de `ResultSet` se queda en el mapper; el caso de uso solo ve un `Order`.

## Punto clave para recordar

> **Al cruzar un límite, traduce los datos.** Cada capa tiene su propia estructura y un Mapper convierte entre ellas. Así nunca dejas que la forma de la base de datos o de la UI se filtre hacia las reglas de negocio, y la dependencia sigue apuntando siempre hacia adentro.

---

Anterior: [← Gateways](./03-gateways.md) · Volver al [índice del tópico](./README.md)
