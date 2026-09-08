# 4. Datos que cruzan fronteras

## La regla sobre los datos

Cuando un dato cruza un límite entre anillos, debe hacerlo en la forma **más simple y aislada** posible.

> Lo que cruza las fronteras son **estructuras de datos simples** (DTOs / *request* y *response models*): objetos planos, sin comportamiento ligado a un anillo externo. **Nunca** deben cruzar Entities, ni filas de la base de datos, ni objetos atados a un framework.

```
   Interactor  ──(response model: struct simple)──►  Presenter
   Controller  ──(request model: struct simple)───►  Interactor

   PROHIBIDO cruzar la frontera con:
     - una Entity de negocio
     - una fila / registro del ORM (row)
     - un objeto del framework (HttpRequest, ResultSet, DataTable...)
```

## Por qué no una Entity ni una fila de DB

Si pasas una fila de la base de datos hacia adentro, un anillo interior terminaría **conociendo** un detalle del anillo exterior (la estructura de la tabla). Eso **viola la Regla de Dependencia**: el interior no debe saber nada del exterior.

```mermaid
flowchart LR
    DB[("Base de datos")] -->|fila / row| REPO["Gateway / Repository<br/>(Interface Adapters)"]
    REPO -->|DTO simple| UC["Use Case (interior)"]

    DB -. NO pasar la fila .-> UC

    linkStyle 2 stroke:#c0392b,stroke-dasharray:5 5
```

El repositorio **traduce** la fila a una estructura simple antes de entregarla al caso de uso. Así el interior nunca ve el formato del ORM.

## Forma recomendada de los datos que cruzan

```mermaid
flowchart TB
    RM["Request Model<br/>(DTO de entrada)"] --> UC["Use Case Interactor"]
    UC --> RSM["Response Model<br/>(DTO de salida)"]
    RSM --> PR["Presenter"]

    note1["Objetos planos, inmutables si es posible,<br/>sin dependencias hacia frameworks"]
    RM -.-> note1
    RSM -.-> note1
```

## Comparativa: qué cruza y qué no

| ¿Cruza la frontera? | Ejemplo | ¿Permitido? |
|---------------------|---------|-------------|
| DTO / request model | `CrearPedidoRequest { itemId, cantidad }` | Sí |
| DTO / response model | `PedidoResponse { total, estado }` | Sí |
| Estructura de datos simple | mapa, registro plano, tupla | Sí |
| Entity de negocio | `Pedido` con reglas y métodos | No |
| Fila / registro del ORM | `PedidoRow`, `ResultSet` | No |
| Objeto del framework | `HttpServletRequest`, `DataTable` | No |

## Regla práctica

```
   Cada frontera define SU PROPIO formato de datos.
   Al cruzar:  traduce  ->  entrega una estructura simple  ->  el otro lado nunca ve el formato original.
```

De esta forma, un cambio en la base de datos o en el framework se queda contenido en el anillo externo: solo cambia la traducción, no las estructuras que viajan hacia adentro ni las reglas de negocio.

## Punto clave para recordar

> **Por las fronteras solo viajan DTOs y estructuras simples y aisladas.** Nunca Entities ni objetos atados a la base de datos o al framework, porque eso obligaría al interior a conocer al exterior y rompería la Regla de Dependencia.

---

Anterior: [← Cruce de límites e inversión](./03-cruce-de-limites-e-inversion.md) · Volver al índice: [Tópico 8 — La arquitectura limpia](./README.md)
