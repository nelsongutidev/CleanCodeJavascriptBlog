---
title: "Principios Solid"
description: "SOLID Principles"
date: 2019-08-12
order: 6
lang: "es"
---

## **SOLID**

### Principio de Responsabilidad Única (SRP)

Como se cita en _Código Limpio_, "No debería haber nunca más de un motivo para
que una clase cambie". Es muy tentador acribillar a una clase con un montón de
funcionalidad. El problema que tiene esto, es que tu clase no tendrá cohesión
y tendrá bastantes motivos por los que cambiar. Es por eso que es importante
reducir el número de veces que tendrás que modificar una clase. Y lo es, porque
en caso de que tengamos una clase que haga más de una cosa y modifiquemos una
de ellas, no podemos saber que efectos colaterales puede tener esta acción en las demás.

Mal:

```javascript
class OpcionesUsuario {
  constructor(usuario) {
    this.usuario = usuario;
  }

  changeSettings(opciones) {
    if (this.verificarCredenciales()) {
      // ...
    }
  }

  verificarCredenciales() {
    // ...
  }
}
```

Bien:

```javascript
class AutenticationUsuario {
  constructor(usuario) {
    this.usuario = usuario;
  }

  verificarCredenciales() {
    // ...
  }
}

class UserSettings {
  constructor(usuario) {
    this.usuario = usuario;
    this.autenticacion = new AutenticationUsuario(usuario);
  }

  changeSettings(settings) {
    if (this.autenticacion.verificarCredenciales()) {
      // ...
    }
  }
}
```

### Principio de abierto/cerrado (OCP)

Citado por Bertrand Meyer: _"Las entidades de software (clases, módulos, funciones, ...)
deberían estar abiertas a extensión pere cerradas a modificación."_ ¿Qué significa esto?
Básicamente significa que los usuarios deberían de ser capaces de añadir funcionalidad
a la aplicación sin tener que tocar el código creado hasta ahora.

Mal:

```javascript
class AdaptadorAjax extends Adaptador {
  constructor() {
    super();
    this.name = "adaptadorAjax";
  }
}

class AdaptadorNodos extends Adaptador {
  constructor() {
    super();
    this.nombre = "adaptadorNodos";
  }
}

class  {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.nombre === "adaptadorAjax") {
      return hacerLlamadaAjax(url).then(respuesta => {
        // transformar la respuesta y devolverla
      });
    } else if (this.adapter.nombre === "adaptadorHttpNodos") {
      return hacerLlamadaHttp(url).then(respuesta => {
        // transformar la respuesta y devolverla
      });
    }
  }
}

function hacerLlamadaAjax(url) {
  // request and return promise
}

function hacerLlamadaHttp(url) {
  // request and return promise
}
```

Bien:

```javascript
class AdaptadorAjax extends Adapter {
  constructor() {
    super();
    this.nombre = "adaptadorAjax";
  }

  pedir(url) {
    // Pedir y devolver la promesa
  }
}

class AdaptadorNodos extends Adapter {
  constructor() {
    super();
    this.nombre = "adaptadorNodos";
  }

  pedir(url) {
    // Pedir y devolver la promesa
  }
}

class EjecutadorPeticionesHttp {
  constructor(adaptador) {
    this.adaptador = adaptador;
  }

  fetch(url) {
    return this.adaptador.pedir(url).then(respuesta => {
      // Transformar y devolver la respuesta
    });
  }
}
```

### Principio de sustitución de Liskov (LSP)

Este es un término que asusta para lo sencillo que es. Estrictamente se define como
"Si S es un subtipo de T, entonces los objetos del tipo T deberían poderse substituir
por objetos del tipo S".

Un ejemplo práctico vien a ser si tenemos una _clase padre_ y una _clase hija_,
entonces ambas han de poderse substituir la una por la otra y viceversa sin recibir
ningún tipo de error o datos erróneos. Un caso práctico es el del cuadrado y el
rectángulo. Geométricamente, un cuadrado es un rectángulo, pero si lo creamos
con una relación "es un" a través de herencia, empezamos a tener problemas...

Mal:

```javascript
class Rectangulo {
  constructor() {
    this.anchura = 0;
    this.altura = 0;
  }

  introducirColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  introducirAnchura(anchura) {
    this.anchura = anchura;
  }

  introducirAltura(altura) {
    this.altura = altura;
  }

  conseguirArea() {
    return this.anchura * this.altura;
  }
}

class Cuadrado extends Rectangulo {
  introducirAnchura(anchura) {
    this.anchura = anchura;
    this.altura = anchura;
  }

  introducirAltura(altura) {
    this.width = altura;
    this.altura = altura;
  }
}

function renderizaRectangulosLargos(rectangulos) {
  rectangulos.forEach(rectangulo => {
    rectangulo.introducirAnchura(4);
    rectangulo.introducirAltura(5);
    const area = rectangulo.conseguirArea(); // MAL: Para el cuadrado devuelve 25 y devería ser 20
    rectangulo.render(area);
  });
}

const rectangulos = [new Rectangulo(), new Rectangulo(), new Cuadrado()];
renderizaRectangulosLargos(rectangulos);
```

Bien:

```javascript
class Forma {
  introducirColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangulo extends Forma {
  constructor(width, height) {
    super();
    this.anchura = anchura;
    this.altura = altura;
  }

  conseguirArea() {
    return this.anchura * this.altura;
  }
}

class Cuadrado extends Forma {
  constructor(distancia) {
    super();
    this.distancia = distancia;
  }

  conseguirArea() {
    return this.distancia * this.distancia;
  }
}

function renderizaRectangulosLargos(shapes) {
  shapes.forEach(shape => {
    const area = shape.conseguirArea();
    shape.render(area);
  });
}

const shapes = [new Rectangulo(4, 5), new Rectangulo(4, 5), new Cuadrado(5)];
renderizaRectangulosLargos(shapes);
```

### Principio de Segregacion de Interfaces (ISP)

Javascript no dispone de interfaces así que no podemos aplicar el principio como
tal. De todas maneras, es importante conceptualmente hablando aunque no tengamos
tipados como tal, pues eso resulta haciendo un código mantenible igualmente.

_ISP_ dice que "los servicios no deberían estar forzados a depender de interfaces
que realmente no usan".

Un buen ejemplo en javascript sería las típicas clases que requieren de un
enormes objetos de configuración. No hacer que los servicios requieran de
grandes cantidades de opciones es beneficioso, porque la gran mayoría del tiempo,
no necesitarán esa configuración. Hacerlos opcionales ayuda a no tener el problema
de "Interaz gorda", en inglés conocido como "fat interface".

Mal:

```javascript
class DOMTraverser {
  constructor(configuraciones) {
    this.configuraciones = configuraciones;
    this.setup();
  }

  preparar() {
    this.nodoRaiz = this.configuraciones.nodoRaiz;
    this.ModuloAnimacion.preparar();
  }

  atravesar() {
    // ...
  }
}

const $ = new DOMTraverser({
  nodoRaiz: document.getElementsByTagName("body"),
  moduloAnimacion() {} // Most of the time, we won't need to animate when traversing.
  // ...
});
```

Bien:

```javascript
class DOMTraverser {
  constructor(configuraciones) {
    this.configuraciones = configuraciones;
    this.opciones = configuraciones.opciones;
    this.preparar();
  }

  preparar() {
    this.nodoRaiz = this.configuraciones.nodoRaiz;
    this.prepararOpciones();
  }

  prepararOpciones() {
    if (this.opciones.moduloAnimacion) {
      // ...
    }
  }

  atravesar() {
    // ...
  }
}

const $ = new DOMTraverser({
  nodoRaiz: document.getElementsByTagName("body"),
  opciones: {
    moduloAnimacion() {}
  }
});
```

### Principio de Inversión de Dependencias (DIP)

_Por favor, no confundir con Inyección de Dependencias._ Mucha gente se piensa
que la "D" de _SOLID_ es de Inyección de Dependencias _(Dependency Inection, DI)._

Este principio nos dice dos cosas básicamente:

1. Módulos de alto nivel no deberían depender de módulos de bajo nivel. Ambos
   deberían depender de abstracciones.
2. Las abstracciones no deberían depender de detalles si no que, los detalles
   deberían depender de abstracciones.

Esto puede ser algo complejo al principio, pero si has trabajado con AngularJS,
has visto de manera indirecta esto con la Inyección de Dependencias. Como
comentaba anteriormente, aunque no son lo mismo, van de la mano. La Inversión de
Dependencías es posible gracias a la Inyección de Dependencias. _DI_ hace posible
que los módulos de alto nivel dependan de abstracciones y no de detalles.

El mayor de los beneficioses la reducción del acoplamiento entre módulos. Cuánto
mayor acoplamiento, mayor dificultad en refactorización.

Como hemos comentado antes, `Javascript` no tiene interfaces así que los contratos
son un poco... así asá. Están en nuestro cabeza y eso debemos tenerlo en cuenta.
Mucha gente usa javascript docs, anotaciones en comentarios justo encima de los
módulos y algunas cosas más. Vamos a ver un ejemplo con `RastreadorDeInventario`.

Mal:

```javascript
class SolicitadorDeInventario {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  pedirArticulo(articulo) {
    // ...
  }
}

class RastreadorDeInventario {
  constructor(articulos) {
    this.articulos = articulos;

    // MAL: Hemos creado una dependencia de una concreción que va atada a una implementación
    // Deberíamos tener pedirArticulos  dependiendo únicamente de un método: 'solicitud'
    this.solicitador = new SolicitadorDeInventario();
  }

  pedirArticulos() {
    this.articulos.forEach(articulo => {
      this.solicitador.pedirArticulo(articulo);
    });
  }
}

const rastreadorDeInventario = new RastreadorDeInventario([
  "manzanas",
  "platanos"
]);
rastreadorDeInventario.pedirArticulos();
```

Bien:

```javascript
class RastreadorDeInventario {
  constructor(articulos, solicitador) {
    this.articulos = articulos;
    this.solicitador = solicitador;
  }

  pedirArticulos() {
    this.articulos.forEach(articulo => {
      this.solicitador.pedirArticulo(articulo);
    });
  }
}

class SolicitadorDeInventarioV1 {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  pedirArticulo(articulo) {
    // ...
  }
}

class SolicitadorDeInventarioV2 {
  constructor() {
    this.REQ_METHODS = ["WS"];
  }

  pedirArticulo(articulo) {
    // ...
  }
}

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.

// Construyendo nuestras dependencias desde fuera e inyectandolas, podríamos
// substituir nuestro Módulo solicitador por uno con websockets o lo que sea
const rastreadorDeInventario = new RastreadorDeInventario(
  ["manzanas", "platanos"],
  new SolicitadorDeInventarioV2()
);
rastreadorDeInventario.pedirArticulos();
```
