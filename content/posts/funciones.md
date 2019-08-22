---
title: "Funciones"
description: "Funciones y Clean Code JS."
date: 2019-08-12
order: 3
lang: "es"
---

## Funciones

### Argumentos de una función (idealmente 2 o menos)

Limitar la cantidad de parámetros de una función es increíblemente importante
porque hacen que _las pruebas_ de tu función sean más sencillas. Tener más de tres
lleva a una locura combinatoria donde tienes que probar toneladas de casos
diferentes con cada argumento por separado.

El caso ideal es usar uno o dos argumentos, tres... deben evitarse si es posible.
Cualquier número superior a eso, debería ser agrupado. Por lo general, si tienes
más de dos argumentos, tu función debe de estar haciendo demasiadas cosas. En los
casos donde no es así, la mayoría de las veces un objeto de nivel superior será
suficiente como un argumento _parámetro objeto_.

Ya que Javascript te permite crear objetos al vuelo sin tener que hacer mucho
código repetitivo en una clase, puedes usar un objeto en caso de estar necesitando
muchos parámetros.

Para indicar que propiedades espera la función, puedes usar las funcionalidades
de desestructuración que nos ofrece ES2015/ES6. Éstas tienen algunas ventajas:

1. Cuando alguien mira la firma de la función, sabe inmediatamente que propiedades
   están siendo usadas
2. La desetructuración también clona los valores primitivos especificados del `objeto argumento`
   pasado a la función. Esto puede servir de ayuda para prevenir efectos adversos.
   _Nota: Los objetos y los arrays que son desestructurados del objeto parámetro NO son clonados._
3. Las herramientas lintera o _linterns_ pueden avisarte de qué propiedades del
   objeto parámetro no están en uso. _Cosa que es imposile sin desestructuración._

**🙅‍ Mal:**

```javascript
function crearMenu(titulo, cuerpo, textoDelBoton, cancelable) {
  // ...
}
```

**👨‍🏫 Bien:**

```javascript
function crearMenu({ titulo, cuerpo, textoDelBoton, cancelable }) {
  // ...
}

crearMenu({
  titulo: "Foo",
  cuerpo: "Bar",
  textoDelBoton: "Baz",
  cancelable: true
});
```

### Las funciones deberían hacer una cosa

De lejos, es la regla más importante en la ingeniería del software. Cuando
las funciones hacen más de una cosa, son difíciles de componer y _testear_
entre otras cosas. Si isolamos las funciones por acciones, éstas pueden ser
modificadas y mantenidas con mayor facilidad y tu código será mucho más limpio.
De toda esta guía... si has de aprender algo, que sea esto. Ya estarás mmuy
por delante de muchos desarrolladores de software.

**🙅‍ Mal:**

```javascript
function enviarCorreoAClientes(clientes) {
  clientes.forEach(cliente => {
    const historicoDelCliente = baseDatos.buscar(cliente);
    if (historicoDelCliente.estaActivo()) {
      enviarEmail(cliente);
    }
  });
}
```

**👨‍🏫 Bien:**

```javascript
function enviarCorreoClientesActivos(clientes) {
  clientes.filter(esClienteActive).forEach(enviarEmail);
}

function esClienteActivo(cliente) {
  const historicoDelCliente = baseDatos.buscar(cliente);
  return historicoDelCliente.estaActivo();
}
```

### Los nombres de las funciones deberían decir lo que hacen

**🙅‍ Mal:**

```javascript
function añadirAFecha(fecha, mes) {
  // ...
}

const fecha = new Date();

// Es difícil saber que se le está añadiendo a la fecha en este caso
añadirAFecha(fecha, 1);
```

**👨‍🏫 Bien:**

```javascript
function añadirMesAFecha(mes, fecha) {
  // ...
}

const fecha = new Date();
añadirMesAFecha(1, fecha);
```

### Las funciones deberían ser únicamente de un nivel de abstracción

Cuando tienes más de un nivel de abstracción, tu función normalmente está
hacicendo demasiado. Separarla en funciones más pequeñas te ayudará a poder
reutilizar código y te facilitará el _testear_ éstas.

**🙅‍ Mal:**

```javascript
function analizarMejorAlternativaJavascript(codigo) {
  const EXPRESIONES_REGULARES = [
    // ...
  ];

  const declaraciones = codigo.split(" ");
  const tokens = [];
  EXPRESIONES_REGULARES.forEach(EXPRESION_REGULAR => {
    declaraciones.forEach(declaracion => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach(token => {
    // lex...
  });

  ast.forEach(nodo => {
    // parse...
  });
}
```

**👨‍🏫 Bien:**

```javascript
function analizarMejorAlternativaJavascript(codigo) {
  const tokens = tokenize(codigo);
  const ast = lexer(tokens);
  ast.forEach(nodo => {
    // parse...
  });
}

function tokenize(codigo) {
  const EXPRESIONES_REGULARES = [
    // ...
  ];

  const declaraciones = codigo.split(" ");
  const tokens = [];
  EXPRESIONES_REGULARES.forEach(EXPRESION_REGULAR => {
    declaraciones.forEach(declaracion => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function lexer(tokens) {
  const ast = [];
  tokens.forEach(token => {
    ast.push(/* ... */);
  });

  return ast;
}
```

### Elimina código duplicado

Haz todo lo posible para evitar duplicación de código. Duplicar código es
malo porque significa que para editar un comportamiento... tendrás que modificarlko
en más de un sitio. ¿Y no queremos trabajar de más, verdad?

Como caso práctico: Imagina que tienes un restaurante. Llevas el registro del
inventario: Todos tus tomates, cebollas, ajos, especies, etc... Si tuvieras más
de una lista que tuvieras que actualizar cada vez que sirves un tomate o usas
una especie, sería más fácil de cometer errores, además de todo el tiempo perdido.
Si solo tienes una, la posibilidad de cometer una error se reduce a ésta!

A menudo tienes código duplicado porque tienes dos o más cosas ligeramente
diferentes, que tienen mucho en común, pero sus diferencias te obligan a tener
ese código de más. Borrar la duplicación de código significa crear una abstracción
que pueda manejar este conjunto de cosas diferentes con una sola función/módulo/clase.

Hacer que la abstracción sea correcta es fundamental y a veces bastante complejo.
Es por eso que debes seguir los Principios `SOLID` establecidos en la sección _Clases_.
Las malas abstracciones pueden ser peores que el código duplicado. ¡Así que ten cuidado!
Dicho esto, si se puede hacer una buena abstracción, ¡Házla! Evita repetirte
porque de lo contrario, como hemos comentado anteriormente, te verás editando
en más de un lugar para modificar un comportamiento.

**🙅‍ Mal:**

```javascript
function mostrarListaDesarrolladores(desarrolladores) {
  desarrolladores.forEach(desarrollador => {
    const salarioEsperado = desarrollador.calcularSalarioEsperado();
    const experiencia = desarrollador.conseguirExperiencia();
    const enlaceGithub = desarrollador.conseguirEnlaceGithub();
    const datos = {
      salarioEsperado,
      experiencia,
      enlaceGithub
    };

    render(datos);
  });
}

function mostrarListaJefes(jefes) {
  jefes.forEach(jefe => {
    const salarioEsperado = desarrollador.calcularSalarioEsperado();
    const experiencia = desarrollador.conseguirExperiencia();
    const experienciaLaboral = jefe.conseguirProyectosMBA();
    const data = {
      salarioEsperado,
      experiencia,
      experienciaLaboral
    };

    render(data);
  });
}
```

**👨‍🏫 Bien:**

```javascript
function mostrarListaEmpleados(empleados) {
  empleados.forEach(empleado => {
    const salarioEsperado = empleado.calcularSalarioEsperado();
    const experiencia = empleado.conseguirExperiencia();

    const datos = {
      salarioEsperado,
      experiencia
    };

    switch (empleado.tipo) {
      case "jefe":
        datos.portafolio = empleado.conseguirProyectosMBA();
        break;
      case "desarrollador":
        datos.enlaceGithub = empleado.conseguirEnlaceGithub();
        break;
    }

    render(datos);
  });
}
```

### Asigna objetos por defecto con Object.assign

**🙅‍ Mal:**

```javascript
const configuracionMenu = {
  titulo: null,
  contenido: "Bar",
  textoBoton: null,
  cancelable: true
};

function crearMenu(config) {
  config.titulo = config.titulo || "Foo";
  config.contenido = config.contenido || "Bar";
  config.textoBoton = config.textoBoton || "Baz";
  config.cancelable =
    config.cancelable !== undefined ? config.cancelable : true;
}

crearMenu(configuracionMenu);
```

**👨‍🏫 Bien:**

```javascript
const configuracionMenu = {
  titulo: "Order",
  // El usuario no incluyó la clave 'contenido'
  textoBoton: "Send",
  cancelable: true
};

function crearMenu(configuracion) {
  configuracion = Object.assign(
    {
      titulo: "Foo",
      contenido: "Bar",
      textoBoton: "Baz",
      cancelable: true
    },
    configuracion
  );

  // configuracion ahora es igual a: {titulo: "Order", contenido: "Bar", textoBoton: "Send", cancelable: true}
  // ...
}

crearMenu(configuracionMenu);
```

### No utilices banderas o flags

Las banderas o _flags_ te indican de que esa función hace más de una cosa. Ya
que como vamos repitiendo, nuestras funciones solo deberían hacer una cosa, separa
esa lógica que es diferenciada por la bandera o _flag_ en una nueva función.

**🙅‍ Mal:**

```javascript
function crearFichero(nombre, temporal) {
  if (temporal) {
    fs.create(`./temporal/${nombre}`);
  } else {
    fs.create(nombre);
  }
}
```

**👨‍🏫 Bien:**

```javascript
function crearFichero(nombre) {
  fs.create(nombre);
}

function crearFicheroTemporal(nombre) {
  crearFichero(`./temporal/${nombre}`);
}
```

### Evita los efectos secundarios (parte 1)

Una función produce un efecto adverso/colateral si hace otra cosa que recibir
un parámetro de entrada y retornar otro valor o valores. Un efecto adverso puede
ser escribir un fichero, modificar una variable global o accidentalmente enviar
todo tu dinero a un desconocido.

Ahora bien, a veces necesitamos efectos adversos en nuestros programas. Como
en el ejemplo anterior, quizás necesitas escribir en un fichero. Así pues, lo que
queremos es centralizar donde se hace esta acción. No queremos que esta lógica
la tengamos que escribir en cada una de las funciones o clases que van a utilizarla.
Para eso, la encapsularemos en un servicio que haga eso. Sólo eso.

El objetivo principal es evitar errores comunes como compartir el estado entre objetos
sin ninguna estructura, usando tipos de datos mutables que pueden ser escritos por cualquier cosa
y no centralizar donde se producen sus efectos secundarios. Si puedes hacer esto, serás
más feliz que la gran mayoría de otros programadores.

**🙅‍ Mal:**

```javascript
// Variable Global referenciada por la siguiente función
// Si tuvieramos otra función que usara ese nombre, podría ser un array y lo estaríamos rompiendo
// If we had another function that used this name, now it'd be an array and it could break it.
let nombre = 'Ryan McDermott';

function separarEnNombreYApellido) {
  nombre = nombre.split(' ');
}

separarEnNombreYApellido();

console.log(nombre); // ['Ryan', 'McDermott'];
```

**👨‍🏫 Bien:**

```javascript
function separarEnNombreYApellido) {
  return nombre.split(' ');
}

const nombre = 'Ryan McDermott';
const nuevoNombre = separarEnNombreYApellidoe);

console.log(nombre); // 'Ryan McDermott';
console.log(nuevoNombre); // ['Ryan', 'McDermott'];
```

### Evita los efectos secundarios (parte 2)

En JavaScript, los primitivos se pasan por valor y los objetos / arrays se pasan por
referencia. En el caso de objetos y arrays, si su función hace un cambio como por ejemplo,
añadiendo un elemento al array que representa el carrito de la compra, entonces cualquier
otra función que use ese array `carrito` se verá afectada por esta modificación.
Eso puede ser genial, sin embargo, también puede ser malo. Imaginemos una mala situación:

El usuario hace clic en el botón "Comprar", que llama a una función de "compra" que
genera una petición de red y envía el array `carrito` al servidor. Dada una mala
conexión de red, la función `comprar` tiene que seguir reintentando la solicitud.
Ahora, ¿Qué pasa si mientras tanto el usuario hace clic accidentalmente en el botón
_"Agregar al carrito"_ en un elemento que realmente no quiere, antes de que comience
la solicitud de red? Si esto sucede y la solicitud de red comienza, entonces esa
función de compra enviará el artículo agregado accidentalmente porque tiene una
referencia al objeto dado que la función `añadirObjetoAlCarrito` modificó el `carrito`
agregando un elemento que no deseado.

Una buena solución para `añadirObjetoAlCarrito` podría ser clonar el `carrito`, editarlo,
y retornar la copia. Esto nos asegura que ninguna otra función tiene referencia al
objeto con los campos modificados. Así pues, ninguna otra función se verá afectada
por nuestros cambios.

Dos advertencias que mencionar para este enfoque:

1. Puede haber casos en los que realmente desee modificar el objeto de entrada,
   pero cuando adopte esta práctica de programación encontrará que esos casos son
   bastante raros ¡La mayoría de las cosas se pueden refactorizar para que no tengan
   efectos secundarios!

2. Clonar objetos grandes puede ser muy costosa en términos de rendimiento.
   Por suerte, en la práctica, esto no es un gran problema dado que hay
   [buenas librerías](https://facebook.github.io/immutable-js/) que permiten este
   tipo de enfoque de programación. Es rápido y no requiere tanta memoria como te
   costaría a ti clonar manualmente los arrays y los objetos.

**🙅‍ Mal:**

```javascript
const añadirObjetoAlCarrito = (carrito, objeto) => {
  carrito.push({ objeto, fecha: Date.now() });
};
```

**👨‍🏫 Bien:**

```javascript
const añadirObjetoAlCarrito = (carrito, objeto) => {
  return [...carrito, { objeto, fecha: Date.now() }];
};
```

### No escribas en variables globales

La contaminación global es una mala práctica en JavaScript porque podría chocar
con otra librería y usuarios usuarios de tu API no serían conscientes de ello hasta
que tuviesen un error en producción. Pensemos en un ejemplo: ¿Qué pasaría si quisieras
extender los arrays de Javascript para tener un método `diff` que pudiera enseñar la
diferencia entre dos arrays? Podrías escribir tu nueva función en el `Array.prototype`,
pero podría chocar con otra librería que intentó hacer lo mismo. ¿Qué pasa si esa otra
librería estaba usando `diff` para encontrar la diferencia entre los elementos primero
y último de una matriz? Tendríamos problemas... Por eso, sería mucho mejor usar las
clases ES2015 / ES6 y simplemente extender el `Array` global.

**🙅‍ Mal:**

```javascript
Array.prototype.diff = function diff(matrizDeComparación) {
  const hash = new Set(matrizDeComparación);
  return this.filter(elemento => !hash.has(elemento));
};
```

**👨‍🏫 Bien:**

```javascript
class SuperArray extends Array {
  diff(matrizDeComparación) {
    const hash = new Set(matrizDeComparación);
    return this.filter(elemento => !hash.has(elemento));
  }
}
```

### Da prioridad a la programación funcional en vez de la programación imperativa

Javascript no es un lenguage funcional en la misma medida que lo es Haskell, pero
tiene aspectos que lo favorecen. Los lenguages funcionales pueden ser más fáciles
y limpios de _testear_. Favorece este estilo de programación siempre que puedas.

**🙅‍ Mal:**

```javascript
const datosSalidaProgramadores = [
  {
    nombre: "Uncle Bobby",
    liniasDeCodigo: 500
  },
  {
    nombre: "Suzie Q",
    liniasDeCodigo: 1500
  },
  {
    nombre: "Jimmy Gosling",
    liniasDeCodigo: 150
  },
  {
    nombre: "Gracie Hopper",
    liniasDeCodigo: 1000
  }
];

let salidaFinal = 0;

for (let i = 0; i < datosSalidaProgramadores.length; i++) {
  salidaFinal += datosSalidaProgramadores[i].liniasDeCodigo;
}
```

**👨‍🏫 Bien:**

```javascript
const datosSalidaProgramadores = [
  {
    nombre: "Uncle Bobby",
    liniasDeCodigo: 500
  },
  {
    nombre: "Suzie Q",
    liniasDeCodigo: 1500
  },
  {
    nombre: "Jimmy Gosling",
    liniasDeCodigo: 150
  },
  {
    nombre: "Gracie Hopper",
    liniasDeCodigo: 1000
  }
];

const salidaFinal = datosSalidaProgramadores
  .map(salida => salida.linesOfCode)
  .reduce((totalLinias, linias) => totalLinias + linias);
```

### Encapsula los condicionales

**🙅‍ Mal:**

```javascript
if (fsm.state === "cogiendoDatos" && estaVacio(listaNodos)) {
  // ...
}
```

**👨‍🏫 Bien:**

```javascript
function deberiaMostrarSpinner(fsm, listaNodos) {
  return fsm.state === "cogiendoDatos" && estaVacio(listaNodos);
}

if (deberiaMostrarSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

### Evita condicionales negativos

**🙅‍ Mal:**

```javascript
function noEstaElNodoPresente(node) {
  // ...
}

if (!noEstaElNodoPresente(node)) {
  // ...
}
```

**👨‍🏫 Bien:**

```javascript
function estaElNodoPresente(node) {
  // ...
}

if (estaElNodoPresente(node)) {
  // ...
}
```

### Evita condicionales

Esto parece una tarea imposible. Al escuchar esto por primera vez, la mayoría de
la gente dice _"¿como voy a ser capaz de hacer cosas sin un `if`"?_ La respuesta a eso,
es que deberías usar polimorfismo para conserguir lo mismo en la gran mayoría de los
casos. La segunda pregunta que normalmente la gente hace es, _¿Bueno está bien pero
para que voy a querer hacerlo?_ La respuesta es uno de los conceptos previos que
hemos visto de _Código limpio_: Una función debería hacer únicamente una cosa.
Cuando tienes una función o clase que posee un `if`, le estás diciendo al usuario
que tu función está haciendo más de una cosa. Recuerda, tan sólo una cosa.

**🙅‍ Mal:**

```javascript
class Avion {
  // ...
  obtenerAlturaDeVuelo() {
    switch (this.tipo) {
      case "777":
        return this.cogerAlturaMaxima() - this.conseguirNumeroPasajeros();
      case "Air Force One":
        return this.cogerAlturaMaxima();
      case "Cessna":
        return this.cogerAlturaMaxima() - this.getFuelExpenditure();
    }
  }
}
```

**👨‍🏫 Bien:**

```javascript
class Avion {
  // ...
}

class Boeing777 extends Avion {
  // ...
  obtenerAlturaDeVuelo() {
    return this.cogerAlturaMaxima() - this.conseguirNumeroPasajeros();
  }
}

class AirForceOne extends Avion {
  // ...
  obtenerAlturaDeVuelo() {
    return this.cogerAlturaMaxima();
  }
}

class Cessna extends Avion {
  // ...
  obtenerAlturaDeVuelo() {
    return this.cogerAlturaMaxima() - this.getFuelExpenditure();
  }
}
```

### Evita el control de tipos (parte 1)

Javascript es un lenguaje no tipado. Esto significa que las funciones pueden recibir
cualquier tipo como argumento. A veces, nos aprovechamos de eso... y es por eso, que
se vuelve muy tentador el controlar los tipos de los argumentos de la función. Hay
algunas soluciones para evitar esto. La primera, son APIs consistentes. Por API se
entiende de que manera nos comunicamos con ese módulo/función.

**🙅‍ Mal:**

```javascript
function viajarATexas(vehiculo) {
  if (vehiculo instanceof Bicicleta) {
    vehiculo.pedalear(this.ubicacionActual, new Localizacion("texas"));
  } else if (vehiculo instanceof Car) {
    vehiculo.conducir(this.ubicacionActual, new Localizacion("texas"));
  }
}
```

**👨‍🏫 Bien:**

```javascript
function viajarATexas(vehiculo) {
  vehiculo.mover(this.ubicacionActual, new Localizacion("texas"));
}
```

### Evita control de tipos (parte 2)

Si estás trabajando con los tipos primitivos como son las `cadenas` o `enteros`,
y no puedes usar polimorfismo pero aún ves la necesidad del control de tipos,
deberías considerar `Typescript`. Es una excelente alternativa al `Javascript`
convencional que nos aporta control de tipos de manera estática entre otras
muchas cosas. El problema de controlar manualmente el tipado en `Javascript` es
que para hacerlo bien, necesitamos añadir mucho código a bajo nivel que afecta a
la legibilidad del código. Mantén tu código `Javascript` limpio, escribe _tests_
y intenta tener revisiones de código. Si no, intenta cubrir el máximo de cosas con
`Typescript` que como ya hemos dicho, es una muy buena alternativa.

**🙅‍ Mal:**

```javascript
function combina(valor1, valor2) {
  if (
    (typeof valor1 === "number" && typeof valor2 === "number") ||
    (typeof valor1 === "string" && typeof valor2 === "string")
  ) {
    return valor1 + valor2;
  }

  throw new Error("Debería ser una cadena o número");
}
```

**👨‍🏫 Bien:**

```javascript
function combina(valor1, valor2) {
  return valor1 + valor2;
}
```

### No optimizes al máximo

Los navegadores modernos hacen mucha optimización por detrás en tiempo de ejecución.
Muchas veces, al interntar optimizar tu código... estás perdiendo el tiempo.
[Esta es una buena documentación](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)
para ver donde falta optimización. Pon el foco en éstas hasta que estén arregladas/hechas
si es que se pueden.

**🙅‍ Mal:**

```javascript
// En los navegadores antiguos, cada iteración en la que `list.length` no esté cacheada
// podría ser costosa por el recálculo de este valor. En los modernos, ya está optimizado
for (let i = 0, tamaño = lista.length; i < tamaño; i++) {
  // ...
}
```

**👨‍🏫 Bien:**

```javascript
for (let i = 0; i < lista.length; i++) {
  // ...
}
```

### Borra código inútil

El código inútil es tan malo como la duplicación. No hay razón alguna para
mantenerlo en tu código. Si no está siendo usado por nadie, ¡Bórralo! Siempre
estará disponible en sistema de versiones para el caso que lo necesites.

**🙅‍ Mal:**

```javascript
function antiguoModuloDePeticiones(url) {
  // ...
}

function nuevoModuloDePeticiones(url) {
  // ...
}

const peticion = nuevoModuloDePeticiones;
calculadorDeInventario("manzanas", peticion, "www.inventory-awesome.io");
```

**👨‍🏫 Bien:**

```javascript
function nuevoModuloDePeticiones(url) {
  // ...
}

const peticion = nuevoModuloDePeticiones;
calculadorDeInventario("manzanas", peticion, "www.inventory-awesome.io");
```
