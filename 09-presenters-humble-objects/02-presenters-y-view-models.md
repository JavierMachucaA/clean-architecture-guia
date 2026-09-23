# 2. Presenters y View Models

## El caso más claro de Humble Object

La interfaz de usuario es difícil de testear: hay que arrancar la pantalla, mover el ratón, leer píxeles. Por eso se aplica el Humble Object Pattern justo ahí y nacen dos piezas:

- **Presenter**: objeto **testeable**. Toma el resultado del caso de uso y lo transforma en un **View Model**.
- **View**: objeto **humilde**. No decide nada; solo mueve los campos del View Model a los widgets.

> El trabajo de la View se reduce a copiar datos del View Model a la pantalla. La View no procesa esos datos de ninguna forma.

## Qué es un View Model

El **View Model** es una estructura de datos plana con **todo ya resuelto**: textos formateados, banderas de estado, colores como strings, listas listas para pintar. No tiene lógica.

```mermaid
flowchart LR
    UC["⚙️ Use case<br/>(resultado)"] ==> P["🧠 Presenter<br/>(decide y formatea)"]
    P ==> VM["📦 View Model<br/>(datos ya cocinados)"]
    VM ==> V["🖥️ View<br/>(solo pinta)"]

    style UC fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style P fill:#6a1b9a,stroke:#ce93d8,color:#fff,stroke-width:2px
    style VM fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    style V fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
```

Todo lo que requiere una **decisión** ocurre en el Presenter, que sí se puede probar. La View recibe el plato terminado.

## Flujo completo

```mermaid
flowchart LR
    UC["⚙️ Use case"] ==>|Output Data| P["🧠 Presenter<br/>(testeable)"]
    P ==>|llena| VM["📦 View Model<br/>(datos planos)"]
    VM ==>|lee| V["🖥️ View<br/>(humilde)"]
    V ==> S["Screen"]

    style UC fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style P fill:#6a1b9a,stroke:#ce93d8,color:#fff,stroke-width:2px
    style VM fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    style V fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style S fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
```

El Presenter transforma un objeto de salida (Output Data) en un View Model; la View simplemente lo muestra.

## Qué decide el Presenter (y la View no)

| Decisión | Responsable |
|----------|-------------|
| Formatear una fecha como `"22/06/2017"` | Presenter |
| Convertir un número a `"$1.200,50"` | Presenter |
| Poner un botón en gris cuando algo está deshabilitado | Presenter (bandera en el VM) |
| Elegir el texto de un mensaje de error | Presenter |
| Colocar ese texto en la etiqueta correspondiente | View |
| Dibujar la etiqueta en pantalla | View |

Nada en la columna "Presenter" toca la pantalla; nada en la columna "View" toma decisiones.

## Ejemplo

❌ La View calcula y formatea (no testeable sin GUI):

```java
class OrderView {
    void render(Order order) {
        date.setText(new SimpleDateFormat("dd/MM/yyyy").format(order.getDate()));
        total.setText("$" + String.format("%.2f", order.getTotal()));
        button.setEnabled(order.getStatus() == PAID); // decision in the view
    }
}
```

✅ El Presenter prepara un View Model; la View solo lo copia:

```java
// Testable
class OrderPresenter {
    OrderViewModel present(OrderOutput out) {
        OrderViewModel vm = new OrderViewModel();
        vm.date = new SimpleDateFormat("dd/MM/yyyy").format(out.date);
        vm.total = "$" + String.format("%.2f", out.total);
        vm.buttonEnabled = out.status == PAID;
        return vm;
    }
}

// Humble
class OrderView {
    void render(OrderViewModel vm) {
        date.setText(vm.date);
        total.setText(vm.total);
        button.setEnabled(vm.buttonEnabled);
    }
}
```

Con esto, `OrderPresenter` se prueba con asserts sobre el View Model, sin abrir una sola ventana.

## En qué capa vive cada pieza

```mermaid
flowchart TD
    subgraph Interna["Interface Adapters"]
        P["🧠 Presenter"]
        VM["📦 View Model"]
    end
    subgraph Externa["Frameworks & Drivers"]
        V["🖥️ View (GUI framework)"]
    end
    UC["⚙️ Use cases"] ==> P
    P ==> VM
    V ==>|depende de| VM

    style P fill:#6a1b9a,stroke:#ce93d8,color:#fff,stroke-width:2px
    style VM fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    style V fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style UC fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
```

El Presenter y el View Model quedan del lado interno (adaptadores de interfaz); la View, atada al framework gráfico, queda en el borde y depende hacia adentro.

## Punto clave para recordar

> **El Presenter piensa, la View pinta.** Toda decisión (formato, color, habilitado/deshabilitado, mensajes) se resuelve en el Presenter y se guarda en un View Model plano. La View es humilde: solo copia esos datos a la pantalla, así que casi no hay nada frágil que testear.

---

Anterior: [← Humble Object Pattern](./01-humble-object-pattern.md) · Siguiente: [Gateways →](./03-gateways.md)
