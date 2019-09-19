---
title: "Testing (Pruebas)"
description: ""
date: 2019-08-12
order: 7
lang: "es"
---

## **Testing**

El testing es más importante que la entrega. Si no tienes test o tienes muchos
que no soy de gran ayuda, cada vez que quieras entregar valor no estarás seguro
de ue eso funciona debidamente y que nada falla. Puedes decidir con el equipo
cuál es el porcentaje al que queréis ceñiros pero, la única manera de tener
confianza total de que nada falla, es teniendo 100% de covertura de test. Para
esto, ncesitarás tener una gran herramienta para poder testear pero también
una que te calcule adecuadamente [el porcentaje cubierto](http://gotwarlost.github.io/istanbul/).

No hay excusas para no escribir tests. Hay
[un montón de frameworks de JS](http://jstherightway.org/#testing-tools) entre
los que podréis tu y tu equipo decidir. Una vez hayáis elegido el framework,
para cada nueva funcionalidad que se quiera añadir a la plataforma, escribir tests.
Si prefieres hacer _Test-Driven Development_ me parece bien, pero la ide principal de
los test es dar confianza suficiente al programador para que pueda seguir entregando valor.

### Sólo un concepto por test

Mal:

```javascript
import assert from "assert";

describe("MakeMomentJSGreatAgain", () => {
  it("maneja límites de las fechas", () => {
    let fecha;

    fecha = new MakeMomentJSGreatAgain("1/1/2015");
    fecha.addDays(30);
    assert.equal("1/31/2015", fecha);

    fecha = new MakeMomentJSGreatAgain("2/1/2016");
    fecha.addDays(28);
    assert.equal("02/29/2016", fecha);

    fecha = new MakeMomentJSGreatAgain("2/1/2015");
    fecha.addDays(28);
    assert.equal("03/01/2015", fecha);
  });
});
```

Bien:

```javascript
import assert from "assert";

describe("MakeMomentJSGreatAgain", () => {
  it("Maneja los meses con 30 días", () => {
    const fecha = new MakeMomentJSGreatAgain("1/1/2015");
    fecha.addDays(30);
    assert.equal("1/31/2015", fecha);
  });

  it("Maneja los años bisiestos", () => {
    const fecha = new MakeMomentJSGreatAgain("2/1/2016");
    fecha.addDays(28);
    assert.equal("02/29/2016", fecha);
  });

  it("Maneja los años NO bisiestos", () => {
    const fecha = new MakeMomentJSGreatAgain("2/1/2015");
    fecha.addDays(28);
    assert.equal("03/01/2015", fecha);
  });
});
```
