# 1. Humble Object Pattern

## El problema: comportamientos difíciles de testear

En los bordes de un sistema siempre aparece código que es **muy difícil de probar con tests unitarios**: pintar una GUI, hablar con hardware, ejecutar SQL, leer de la red. No porque tenga mucha lógica, sino porque depende del entorno.

La tentación es mezclar esa lógica frágil con las reglas importantes. El resultado es que **nada** queda testeable, porque todo cuelga del detalle técnico.

> El **Humble Object Pattern** es una forma de separar los comportamientos difíciles de testear de los comportamientos fáciles de testear.

## La solución: partir en dos objetos

La idea es dividir la frontera en dos piezas:

1. Un objeto **humilde** (*humble*): contiene solo el código que no se puede probar. Es lo más pequeño y tonto posible; no toma decisiones.
2. Un objeto **comprobable** (*testable*): contiene toda la lógica que se sacó del objeto humilde. Aquí vive lo que realmente importa y se prueba a fondo.

```mermaid
flowchart LR
    subgraph Frontera["Frontera del sistema"]
        T["🧠 Testable object<br/>(all the logic)"]
        H["🖥️ Humble object<br/>(no logic,<br/>only the detail)"]
    end
    T ==>|le entrega datos listos| H
    H ==>|toca| DET["GUI / hardware / DB / network"]

    style T fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style H fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style DET fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
```

El objeto testeable prepara el trabajo; el objeto humilde solo lo ejecuta contra el mundo real.

## Cómo se ve el reparto

```mermaid
flowchart LR
    subgraph Frontera["FRONTERA"]
        direction LR
        T["🧠 TESTEABLE<br/>• decide qué mostrar<br/>• calcula, formatea<br/>• aplica reglas<br/>• 90% del código<br/>• cubierto por tests"]
        H["🖥️ HUMILDE<br/>• pinta píxeles<br/>• lee/escribe el detalle<br/>• 10% del código<br/>• casi sin tests"]
    end

    style T fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style H fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
```

La meta es empujar hacia el objeto humilde la **menor cantidad de código posible**: solo lo que es imposible de probar.

## Dónde aparece este patrón

| Frontera | Objeto humilde (difícil de testear) | Objeto testeable (la lógica) |
|----------|-------------------------------------|------------------------------|
| Interfaz de usuario | View | Presenter |
| Base de datos | Implementación del Gateway (SQL) | Caso de uso + interfaz Gateway |
| Servicios externos | Cliente HTTP concreto | Lógica que arma la petición |
| Hardware / dispositivos | Driver del dispositivo | Controlador con las reglas |

El patrón se repite en cada límite: el detalle queda humilde, la política queda comprobable.

## Ejemplo: mezclar vs separar

❌ Todo junto, imposible de testear sin la pantalla real:

```java
class BalanceView {
    void show(Account account) {
        double balance = account.getBalance();
        String text = (balance < 0 ? "-$" : "$") + Math.abs(balance); // logic hidden in the view
        label.setText(text);        // real drawing (not testable)
        label.setColor(balance < 0 ? RED : BLACK);
    }
}
```

✅ Lógica en un objeto testeable, la vista queda humilde:

```java
// Testable: decides text and color, draws nothing
class BalancePresenter {
    BalanceViewModel prepare(Account account) {
        double balance = account.getBalance();
        String text = (balance < 0 ? "-$" : "$") + Math.abs(balance);
        String color = balance < 0 ? "RED" : "BLACK";
        return new BalanceViewModel(text, color);
    }
}

// Humble: only paints what is already decided
class BalanceView {
    void show(BalanceViewModel vm) {
        label.setText(vm.text);
        label.setColor(vm.color);
    }
}
```

Ahora `BalancePresenter` se prueba con simples asserts, sin arrancar la interfaz gráfica.

## Por qué también es un boundary arquitectónico

El objeto humilde y el testeable suelen quedar en **capas distintas**, con la dependencia apuntando hacia adentro. El Humble Object no es solo un truco de testeo: es una manera de dibujar y reforzar un límite arquitectónico.

```mermaid
flowchart TD
    H["🖥️ Humble object<br/>(capa externa)"] ==>|depende de| I["📦 Interface / View Model<br/>(capa interna)"]
    T["🧠 Testable object<br/>(capa interna)"] ==>|implementa/usa| I

    style H fill:#37474f,stroke:#90a4ae,color:#fff,stroke-width:2px
    style I fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:2px
    style T fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
```

## Punto clave para recordar

> **Separa lo que puedes probar de lo que no.** Deja en el objeto humilde solo el detalle imposible de testear (dibujar, hablar con hardware o BD) y mueve toda la lógica al objeto comprobable. Así maximizas la parte del sistema cubierta por tests.

---

Siguiente: [Presenters y View Models →](./02-presenters-y-view-models.md)
