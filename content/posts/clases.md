---
title: "Clases"
description: "Prioriza las classes de ES2015/ES6 antes que las funciones planas de ES50"
date: 2019-08-12
order: 5
lang: "es"
---

## Clases

### Prioriza las classes de ES2015/ES6 antes que las funciones planas de ES50

Es muy complicado de conseguir que un código sea entendible y fácil de leer con
herencia de clases, construcción y metodos típicos de clases con las clases de ES5.
Si necesitas herencia (y de seguro, que no la necesitas) entonces, dale prioridad a
las clases ES2015/ES6. De todas las maneras, deberías preferir pequeñas funciones
antes que ponerte a hacer clases. Solo cuando tengas un código largo o cuando veas
necesaria la implementación de clases, añádelas.

Mal:

```javascript
const Animal = function(edad) {
  if (!(this instanceof Animal)) {
    throw new Error("Inicializa Animal con `new`");
  }

  this.edad = edad;
};

Animal.prototype.mover = function mover() {};

const Mamifero = function(edad, furColor) {
  if (!(this instanceof Mamifero)) {
    throw new Error("Inicializa Mamifero con `new`");
  }

  Animal.call(this, edad);
  this.furColor = furColor;
};

Mamifero.prototype = Object.create(Animal.prototype);
Mamifero.prototype.constructor = Mamifero;
Mamifero.prototype.aniversario = function aniversario() {};

const Humano = function(edad, furColor, idioma) {
  if (!(this instanceof Humano)) {
    throw new Error("Inicializa Humano con `new`");
  }

  Mamifero.call(this, edad, furColor);
  this.idioma = idioma;
};

Humano.prototype = Object.create(Mamifero.prototype);
Humano.prototype.constructor = Humano;
Humano.prototype.hablar = function hablar() {};
```

Bien:

```javascript
class Animal {
  constructor(edad) {
    this.edad = edad;
  }

  mover() {
    /* ... */
  }
}

class Mamifero extends Animal {
  constructor(edad, furColor) {
    super(edad);
    this.furColor = furColor;
  }

  aniversario() {
    /* ... */
  }
}

class Human extends Mamifero {
  constructor(edad, furColor, idioma) {
    super(edad, furColor);
    this.idioma = idioma;
  }

  hablar() {
    /* ... */
  }
}
```

### Utiliza el anidación de funciones

Este es un patrón útil en Javascript y verás que muchas librerías como jQuery
o Lodash lo usan. Permite que tu código sea expresivo y menos verboso.
Por esa razón, utiliza las funciones anidadas y date cuenta de que tan limpio estará
tu código. En las funciones de tu clase, sencillamente retorna `this` al final de
cada una y con eso, tienes todo lo necesario pra poder anidar las llamadas a las
funciones.

Mal:

```javascript
class Coche {
  constructor(marca, modelo, color) {
    this.marca = marca;
    this.modelo = modelo;
    this.color = color;
  }

  introducirMarca(marca) {
    this.marca = marca;
  }

  introducirModelo(modelo) {
    this.modelo = modelo;
  }

  introducirColor(color) {
    this.color = color;
  }

  guardar() {
    console.log(this.marca, this.modelo, this.color);
  }
}

const coche = new Coche("Ford", "F-150", "rojo");
coche.introducirColor("rosa");
coche.guardar();
```

Bien:

```javascript
class Coche {
  constructor(marca, modelo, color) {
    this.marca = marca;
    this.modelo = modelo;
    this.color = color;
  }

  introducirMarca(marca) {
    this.marca = marca;
    // NOTE: Retornamos this para poder anidas funciones
    return this;
  }

  introducirModelo(modelo) {
    this.modelo = modelo;
    // NOTE: Retornamos this para poder anidas funciones
    return this;
  }

  introducirColor(color) {
    this.color = color;
    // NOTE: Retornamos this para poder anidas funciones
    return this;
  }

  guardar() {
    console.log(this.marca, this.modelo, this.color);
    // NOTE: Retornamos this para poder anidas funciones
    return this;
  }
}

const coche = new Coche("Ford", "F-150", "rojo")
  .introducirColor("rosa")
  .guardar();
```

### Prioriza la composición en vez de la herecia

Como se citó en [_Patrones de Diseño_](https://en.wikipedia.org/wiki/Design_Patterns)
por "the Gang of Four", deberías priorizar la composición en vez de la herecia
siempre que puedas. Hay muy buenas razones para usar tanto la herecia como la
composición. El problema principal es que nuestra mente siempre tiende a la herencia
como primera opción, pero deberíamos de pensar qué tan bien nos encaja la composición
en ese caso particular porque en muchas ocasiones es lo más acertado.

Te estarás preguntando entonces, _¿Cuando debería yo usar la herencia?_ Todo depende.
Depende del problema que tengas entre mano, pero ya hay ocasiones particulares donde
la herencia tiene más sentido que la composición:

1. Tu herencia representa una relación "es un/a" en vez de "tiene un/a"
   (Humano->Animal vs. Usuario->DetallesUsuario)
2. Puedes reutilizar código desde las clases base (Los humanos pueden moverse como animales)
3. Quieres hacer cambios generales a clases derivadas cambiando la clase base.
   (Cambiar el consumo de calorías a todos los animales mientras se mueven)

Mal:

```javascript
class Empleado {
  constructor(nombre, correoElectronico) {
    this.nombre = nombre;
    this.correoElectronico = correoElectronico;
  }

  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Empleado
class InformacionImpuestosEmpleado extends Empleado {
  constructor(ssn, salario) {
    super();
    this.ssn = ssn;
    this.salario = salario;
  }

  // ...
}
```

Bien:

```javascript
class InformacionImpuestosEmpleado {
  constructor(ssn, salario) {
    this.ssn = ssn;
    this.salario = salario;
  }

  // ...
}

class Empleado {
  constructor(nombre, correoElectronico) {
    this.nombre = nombre;
    this.correoElectronico = correoElectronico;
  }

  introducirInformacionImpuestos(ssn, salario) {
    this.informacionImpuestos = new InformacionImpuestosEmpleado(ssn, salario);
  }
  // ...
}
```
