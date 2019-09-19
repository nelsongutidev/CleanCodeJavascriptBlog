---
title: "Formato"
description: ""
date: 2019-08-12
order: 10
lang: "es"
---

## Formato

El formato del código es algo subjetivo. Como otras reglas aquí, no hay una regla
que deberías seguir o una fórmula secreta. Lo que si que está claro es que no
deberíamos discutir ni crear conflictos con nuestros compañeros de trabajo acerca
de estas reglas. Hay unas cuantas [herrmientas](http://standardjs.com/rules.html)
que automatizan todas las reglas de formato de texto. ¡Ahorrarse tiempo en estas
formateando el texto es un pasada!

### Usa consistenemente la capitalización

Como ya hemos dicho, `javascript` es un lenguage no tipado así pues, la
capitalización de las variables importa, y mucho. Estas son reglas totalmente
subjetivas así que como equipo, podéis elegir lo que más os guste/convenga.
La cuestión es que independientemente de lo que decidáis, seáis consistentes.

Mal:

```javascript
const DIAS_POR_SEMANA = 7;
const diasPorMes = 30;

const canciones = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artistas = ["ACDC", "Led Zeppelin", "The Beatles"];

function borrarBaseDeDatos() {}
function restablecer_baseDeDatos() {}

class animal {}
class Alpaca {}
```

Bien:

```javascript
const DIAS_POR_SEMANA = 7;
const DIAS_POR_MES = 30;

const CANCIONES = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const ARTISTAS = ["ACDC", "Led Zeppelin", "The Beatles"];

function borrarBaseDeDatos() {}
function restablecerBaseDeDatos() {}

class Animal {}
class Alpaca {}
```

**[⬆ Volver arriba](#contenido)**

### Funciones que llaman y funciones que son llamadas, deberían estar cerca

Si una función llama a otra, haz que esta función que va a ser llamada esté
lo más cerca posible de la función que la llama. Idealmente, situa siempre
la función que va a ser llamada justo después de la función que la ejecuta.
¿El motivo? Pues normalmente acostumbramos a leer de arriba abajo y tampoco
queremos tener que hacer _scroll_ hasta abajo del todo del ficheor para volver
a subir.

Mal:

```javascript
class RevisionDeRendimiento {
  constructor(empleado) {
    this.empleado = empleado;
  }

  conseguirCompañeros() {
    return db.buscar(this.empleado, "compañeros");
  }

  conseguirJefe() {
    return db.buscar(this.empleado, "jefe");
  }

  conseguirOpinionDeLosCompañeros() {
    const compañeros = this.conseguirCompañeros();
    // ...
  }

  executarRevision() {
    this.conseguirOpinionDeLosCompañeros();
    this.conseguirOpinionDelJefe();
    this.conseguirAutoRevision();
  }

  conseguirOpinionDelJefe() {
    const jefe = this.conseguirJefe();
  }

  conseguirAutoRevision() {
    // ...
  }
}

const review = new RevisionDeRendimiento(empleado);
review.executarRevision();
```

Bien:

```javascript
class RevisionDeRendimiento {
  constructor(empleado) {
    this.empleado = empleado;
  }

  executarRevision() {
    this.conseguirOpinionDeLosCompañeros();
    this.conseguirOpinionDelJefe();
    this.conseguirAutoRevision();
  }

  conseguirOpinionDeLosCompañeros() {
    const compañeros = this.conseguirCompañeros();
    // ...
  }

  conseguirCompañeros() {
    return db.buscar(this.empleado, "compañeros");
  }

  conseguirOpinionDelJefe() {
    const jefe = this.conseguirJefe();
  }

  conseguirJefe() {
    return db.buscar(this.empleado, "jefe");
  }

  conseguirAutoRevision() {
    // ...
  }
}

const review = new RevisionDeRendimiento(empleado);
review.executarRevision();
```
