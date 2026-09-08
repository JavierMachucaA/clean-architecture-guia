# Tópico 1 — Introducción y visión general

> Primer bloque de la guía sobre **Clean Architecture** de Robert C. Martin (Uncle Bob).
> Aquí se establece el *para qué* de la arquitectura antes de entrar en principios y patrones.

## Idea central del tópico

Antes de hablar de círculos, capas, SOLID o boundaries, Uncle Bob dedica el primer bloque del libro a responder una pregunta fundamental:

> **¿Por qué molestarse en tener una buena arquitectura?**

La respuesta corta: porque el objetivo de la arquitectura de software es **minimizar el esfuerzo humano necesario para construir y mantener el sistema**. Todo lo demás (patrones, principios, diagramas) son medios para ese fin.

```
   Mala arquitectura  ─────►  cada feature cuesta más que la anterior  ─────►  el equipo se frena
   Buena arquitectura ─────►  el costo por feature se mantiene estable  ─────►  el equipo avanza
```

## Documentos de este tópico

| # | Documento | Punto que cubre |
|---|-----------|-----------------|
| 1 | [01-diseno-vs-arquitectura.md](./01-diseno-vs-arquitectura.md) | Qué es diseño y qué es arquitectura (no son cosas distintas) |
| 2 | [02-objetivo-de-la-arquitectura.md](./02-objetivo-de-la-arquitectura.md) | El objetivo: minimizar el costo de recursos humanos por comportamiento |
| 3 | [03-caso-de-estudio-productividad.md](./03-caso-de-estudio-productividad.md) | Cómo el desorden arquitectónico frena la productividad |
| 4 | [04-comportamiento-vs-estructura.md](./04-comportamiento-vs-estructura.md) | El dilema de los desarrolladores: comportamiento vs. estructura |
| 5 | [05-la-lucha-por-la-arquitectura.md](./05-la-lucha-por-la-arquitectura.md) | Arquitectura frente a la presión por "sacar features" |

## Diagrama del tópico

```mermaid
mindmap
  root((Introducción<br/>y visión general))
    Diseño = Arquitectura
      Mismo objetivo
      Solo cambia el nivel de detalle
    Objetivo
      Minimizar esfuerzo humano
      Bajo costo por comportamiento
    Caso de estudio
      Productividad cae con el desorden
      Costo por línea sube
    Comportamiento vs Estructura
      Urgente vs Importante
      "Funciona" no basta
    La lucha
      Devs vs Managers
      Defender la estructura
```

## Cómo leer este tópico

1. Empieza por **Diseño vs Arquitectura** para desarmar el falso dualismo entre ambos términos.
2. Continúa con **El objetivo de la arquitectura** para fijar el criterio con el que se juzga todo lo demás.
3. Los tres documentos restantes son el argumento económico y político: por qué descuidar la estructura sale caro y cómo defenderla.

## Referencia

- Robert C. Martin, *Clean Architecture: A Craftsman's Guide to Software Structure and Design*, Prentice Hall, 2017 — Parte I ("Introduction") y capítulos iniciales.
