# 2. Use Cases: Application Business Rules

## Qué es un caso de uso

> Un **Use Case** describe las **reglas de negocio específicas de la aplicación** (*Application-Specific Business Rules*): cómo se **automatiza** el negocio en este sistema concreto.

Mientras la entidad dice *qué* es cierto en el negocio (el interés existe), el caso de uso dice *cómo* la aplicación lo usa: qué datos pide, en qué orden ejecuta los pasos, qué valida a nivel de aplicación y qué devuelve.

Un caso de uso define un **flujo**: la interacción entre el usuario y las entidades para conseguir un objetivo.

## Ejemplo: "Aprobar un préstamo"

```
Caso de uso: Aprobar préstamo
  Entrada:  datos del solicitante, monto pedido
  Reglas de aplicación:
    1. Verificar que el solicitante tenga score >= 650
    2. Verificar que el monto <= límite permitido
    3. Crear la entidad Prestamo y pedirle calcularPagoMensual()
    4. Registrar el préstamo aprobado
  Salida:   préstamo aprobado con su pago mensual, o rechazo con motivo
```

Nota que el **paso 3 usa la entidad** (`Prestamo.calcularPagoMensual()`), pero los pasos 1, 2 y 4 son reglas propias de **esta aplicación**: otro banco podría automatizar el mismo negocio con un flujo distinto.

## El caso de uso orquesta, la entidad calcula

```mermaid
sequenceDiagram
    participant UI as Entrada (detalle)
    participant UC as AprobarPrestamoUseCase
    participant E as Prestamo (Entity)
    participant R as Repositorio (interface)

    UI->>UC: request (datos solicitante, monto)
    UC->>UC: validar score y limites (regla de app)
    UC->>E: new Prestamo(...); calcularPagoMensual()
    E-->>UC: pago mensual
    UC->>R: guardar(prestamo)
    UC-->>UI: response (aprobado + pago mensual)
```

## Dónde vive un caso de uso

```
   [ Detalle: Controller/UI ]
             │  request model
             ▼
   ┌───────────────────────────┐
   │  USE CASE                 │
   │   valida reglas de app    │
   │   coordina el flujo       │
   │        │  usa             │
   │        ▼                  │
   │   [ ENTITY: reglas core ] │
   │        │                  │
   │        ▼  interface       │
   │   [ Repositorio abstracto]│
   └───────────────────────────┘
             │  response model
             ▼
   [ Detalle: Presenter/UI ]
```

## Diferencia entre los dos niveles de reglas

| Aspecto | Entity (empresa) | Use Case (aplicación) |
|---------|------------------|-----------------------|
| Alcance | Todo el negocio | Esta aplicación |
| Existiría sin software | Sí | No (describe la automatización) |
| Ejemplo | Fórmula del interés | Flujo de "aprobar préstamo" |
| Estabilidad | Muy alta | Alta, pero cambia si cambia el proceso |
| Depende de | Nada | De las entidades |

### Ejemplo ❌ — caso de uso que mete reglas de empresa

```java
class AprobarPrestamoUseCase {
    Response ejecutar(Request r) {
        // ❌ La fórmula del interés es regla de EMPRESA, no de la app
        double interes = r.monto * 0.05 * r.plazo;
        // ...
    }
}
```

### Ejemplo ✅ — caso de uso que delega en la entidad

```java
class AprobarPrestamoUseCase {
    Response ejecutar(Request r) {
        if (r.score < 650) return Response.rechazo("Score insuficiente");
        Prestamo p = new Prestamo(r.monto, tasaVigente, r.plazo);
        Dinero pago = p.calcularPagoMensual(); // ✅ regla de empresa en la entidad
        repositorio.guardar(p);
        return Response.aprobado(pago);
    }
}
```

## Punto clave para recordar

> **Un caso de uso contiene las reglas específicas de la aplicación: orquesta el flujo y coordina las entidades, pero delega en ellas las reglas críticas del negocio.** Define qué entra (input) y qué sale (output).

---

Anterior: [← Entities: reglas de empresa](./01-entities-reglas-de-empresa.md) · Siguiente: [Modelos de request/response →](./03-modelos-request-response.md)
