---
title: "Las Variables"
description: "Variables y Clean Code JS."
date: 2019-08-12
order: 2
lang: "es"
---

## Variables

### Utiliza nombres con sentido y de fácil pronunciación para las variables

**🙅‍ Mal:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**👨‍🏫 Bien:**

```javascript
const fechaACtual = moment().format("YYYY/MM/DD");
```

### Utiliza el mismo tipo de vocabulario para el mismo tipo de variables

**🙅‍ Mal:**

```javascript
conseguirInformacionUsuario();
conseguirDatosCliente();
conseguirRegistroCliente();)
```

**👨‍🏫 Bien:**

```javascript
conseguirUsuario();
```

### Utiliza nombres que puedan ser buscados

Leeremos más código del que jamás escribiremos. Es importante que el código que
escribamos sea legible y se puede buscar en él. Al no crear variables que sean
significativas para entender nuestro código... Estamos entorpeciendo a sus lectores.
Haz tus variables sean fáciles de entender y buscar. Herramientas como
[buddy.js](https://github.com/danielstjules/buddy.js) y
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
pueden ayudan a identificar constantes no nombradas.

**🙅‍ Mal:**

```javascript
// Para que cojones sirve 86400000?
setTimeout(blastOff, 86400000);
```

**👨‍🏫 Bien:**

```javascript
// Declaralas como constantes nombradas
const MILISEGUNDOS_POR_DIA = 86400000;

setTimeout(blastOff, MILISEGUNDOS_POR_DIA);
```

### Utiliza variables explicativas

**🙅‍ Mal:**

```javascript
const direccion = "Calle Mallorca, Barcelona 95014";
const expresionRegularCodigoPostalCiudad = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
guardarCP(
  direccion.match(expresionRegularCodigoPostalCiudad)[1],
  direccion.match(expresionRegularCodigoPostalCiudad)[2]
);
```

**👨‍🏫 Bien:**

```javascript
const direccion = "One Infinite Loop, Cupertino 95014";
const expresionRegularCodigoPostalCiudad = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [, ciudad, codigoPostal] =
  direccion.match(expresionRegularCodigoPostalCiudad) || [];
guardarCP(ciudad, codigoPostal);
```

### Evita relaciones mentales

Explícito es mejor que implícito.

**🙅‍ Mal:**

```javascript
const ciudades = ["Barcelona", "Madrid", "Sitges"];
ciudades.forEach(l => {
  hacerAlgo();
  hacerAlgoMas();
  // ...
  // ...
  // ...
  // Espera, para que era `l`?
  dispatch(l);
});
```

**👨‍🏫 Bien:**

```javascript
const ciudades = ["Barcelona", "Madrid", "Sitges"];
ciudades.forEach(direccion => {
  hacerAlgo();
  hacerAlgoMas();
  // ...
  // ...
  // ...
  dispatch(direccion);
});
```

### No añadas contexto innecesario

Si tu nombre de clase/objeto ya dice algo, no lo repitas en tu nombre de variable

**🙅‍ Mal:**

```javascript
const Coche = {
  marcaCoche: "Honda",
  modeloCoche: "Accord",
  colorCoche: "Azul"
};

function pintarCoche(coche) {
  coche.colorCoche = "Rojo";
}
```

**👨‍🏫 Bien:**

```javascript
const Coche = {
  marca: "Honda",
  modelo: "Accord",
  color: "Rojo"
};

function pintarCoche(coche) {
  coche.color = "Rojo";
}
```

### Utiliza argumentos por defecto en vez de circuitos cortos o condicionales

Los argumentos por defecto suelen ser más limpios que los cortocircuitos. Ten
en cuenta que si los usas, solo se asignara ese valor por defectos cuando el
valor del parámetro sea `undefined`. Otros valores "falsos" como `''`, `" "`,
`false`,`null`, `0` y `NaN`, no serán reemplazado por un valor predeterminado
pues se consideran valores como tal.

**🙅‍ Mal:**

```javascript
function crearMicroCerveceria(nombre) {
  const nombreMicroCerveceria = nombre || "Hipster Brew Co.";
  // ...
}
```

**👨‍🏫 Bien:**

```javascript
function crearMicroCerveceria(nombre = "Hipster Brew Co.") {
  // ...
}
```
