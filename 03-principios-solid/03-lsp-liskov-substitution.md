# 3. LSP — El Principio de Sustitución de Liskov

## La definición

Barbara Liskov lo formuló en 1988. La idea, resumida por Uncle Bob:

> **Para construir software con partes intercambiables, esas partes deben adherirse a un contrato que permita sustituir unas por otras.**

Dicho de otra forma: si `S` es un subtipo de `T`, entonces los objetos de tipo `T` deben poder **reemplazarse** por objetos de tipo `S` **sin alterar la corrección** del programa. El usuario del supertipo no debería notar la diferencia.

## Un buen uso del LSP

Piensa en una interfaz `Licencia` con un método `calcularTarifa()`. Dos implementaciones la respetan perfectamente:

```mermaid
flowchart TD
    B["⚙️ Billing<br/>(uses License)"]
    L["🧠 «interface» License<br/>calcFee()"]
    P["✅ PersonalLicense"]
    E["✅ BusinessLicense"]

    B ==> L
    P -.implementa.-> L
    E -.implementa.-> L

    style B fill:#2e7d32,stroke:#a5d6a7,color:#fff,stroke-width:2px
    style L fill:#1565c0,stroke:#90caf9,color:#fff,stroke-width:3px
    style P fill:#6a1b9a,stroke:#ce93d8,color:#fff,stroke-width:2px
    style E fill:#6a1b9a,stroke:#ce93d8,color:#fff,stroke-width:2px
```

La clase `Facturación` funciona con cualquier `Licencia`. Se pueden intercambiar libremente: **LSP se cumple.**

## El ejemplo canónico de violación: cuadrado / rectángulo

Parece natural decir "un cuadrado ES un rectángulo", así que `Cuadrado` hereda de `Rectángulo`. Pero un `Rectángulo` permite fijar alto y ancho **por separado**, y un `Cuadrado` no.

```
   Rectangle                     Square (as a subtype)
   ┌───────────────┐             ┌───────┐
   │               │             │       │
   │   width  !=   │             │ width │  changing the width
   │   height      │             │= height│ also changes the height!
   └───────────────┘             └───────┘
   setWidth / setHeight          setWidth forces setHeight  X
   independent
```

El código que usa un `Rectángulo` asume esto:

```
r.setWidth(5)
r.setHeight(4)
assert(r.area() == 20)   // OK with Rectangle
                         // FAIL with Square -> 16, because setHeight(4) changed width to 4
```

Si le pasas un `Cuadrado` donde se esperaba un `Rectángulo`, **el programa se comporta mal**. El usuario tendría que preguntar "¿de qué tipo eres realmente?", lo que rompe la sustituibilidad.

## LSP no es solo herencia

Uncle Bob insiste en que LSP se aplica a **cualquier** relación de subtipos: herencia, interfaces implementadas, o incluso servicios REST que responden a un mismo contrato. Cuando se viola, el sistema se llena de mecanismos especiales (`if type == X`) para atender los casos que no encajan.

| Situación | ¿Respeta LSP? | Consecuencia |
|-----------|---------------|--------------|
| Subtipos que cumplen el contrato completo | ✅ Sí | Intercambiables, sin `if` de tipo |
| Subtipo que restringe lo permitido (cuadrado) | ❌ No | El cliente necesita casos especiales |
| Servicio REST con un campo distinto | ❌ No | El consumidor mete `if` por proveedor |

> Cada violación de LSP obliga a añadir lógica especial de tipo. Esa lógica es una fuga que contamina y complica el sistema.

## Ejemplo conceptual

### ❌ Mal aplicado — el subtipo rompe el contrato

```
class Rectangle { setWidth(w); setHeight(h); area() }
class Square extends Rectangle {
    setWidth(w){  super.setWidth(w); super.setHeight(w) }   // hidden side effect
    setHeight(h){ super.setWidth(h); super.setHeight(h) }   // breaks the client's assumption
}
```

### ✅ Bien aplicado — respetar el contrato del supertipo

```
interface Shape { area() }

class Rectangle implements Shape { setWidth(w); setHeight(h); area() }
class Square    implements Shape { setSide(l);              area() }
```

`Cuadrado` y `Rectángulo` ya no fingen ser intercambiables: comparten solo lo que de verdad comparten (`area()`). No hay suposiciones traicionadas.

## Punto clave para recordar

> **LSP dice que un subtipo debe ser usable en lugar de su supertipo sin sorpresas.** Cuando un subtipo rompe las suposiciones del cliente (como el cuadrado frente al rectángulo), el diseño se llena de lógica especial y pierde su capacidad de tener partes intercambiables.

---

Anterior: [← OCP — Open-Closed Principle](./02-ocp-open-closed.md)

Siguiente: [ISP — Interface Segregation Principle →](./04-isp-interface-segregation.md)
