---
title: "Comentarios"
description: ""
date: 2019-08-12
order: 11
lang: "es"
---

## Comentarios

### Comenta únicamente la lógica de negocio que es compleja

Los comentarios son una disculpa, no un requerimiento. Supuesatmente se dice
que un buen código debería comentarse por si mismo. Un código perfecto no
está optimizado para la máquina sinó que lo está para la manteniblidad de éste
por un compañero o futuro compañero. Para esto, ha de ser lo más semántico posible.
El código ha de estar escrito para que niños pequeños lo entiendan.

Mal:

```javascript
function hashIt(datos) {
  // El hash
  let hash = 0;

  // Tamaño del string
  const tamaño = datos.length;

  // Iteramos a través de cada carácter de los datos
  for (let i = 0; i < tamaño; i++) {
    // Coger código del caracter
    const char = datos.charCodeAt(i);
    // Crear el hash
    hash = (hash << 5) - hash + char;
    // Convertir a un entero de 32 bits
    hash &= hash;
  }
}
```

Bien:

```javascript
function hashIt(datos) {
  let hash = 0;
  const tamaño = datos.length;

  for (let i = 0; i < tamaño; i++) {
    const caracter = datos.charCodeAt(i);
    hash = (hash << 5) - hash + caracter;

    // Convertir a un entero de 32 bits
    hash &= hash;
  }
}
```

**[⬆ Volver arriba](#contenido)**

### No dejes código comentado en tu repositorio

El control de versiones existe para algo. Si tu motivo o excusa por el que comentar
un código es porque en breves o algun día lo vas a necesitas, eso no me sirve. Ese
código que acabas de borrar consta en alguna de tus versiones de tu código fuente.
Lo que deberías hacer entonces quizás, es usar `git tags`, poner el código de la
tarea en el nombre del commit, etc... Hay muchos truquitos para hacer eso!

Mal:

```javascript
hacerCosas();
// hacerOtrasCosas();
// hacerCosasAunMasRaras();
// estoHaceMaravillas();
```

Bien:

```javascript
hacerCosas();
```

**[⬆ Volver arriba](#contenido)**

### No hagas un diario de comentarios

Recuerda ¡Usa el control de versioens! No hay motivo alguno para tener código
muerto, código comentado y aún menos, un diadrio o resumen de modificaciones en
tus comentarios. Si quieres ver las modificaciones, usa `git log`, la herramiento
`blame` o incluso el `history`.

Mal:

```javascript
/**
 * 2016-12-20: `monads` borrados, no hay quien los entienda
 * 2016-10-01: Código mejorado con 'monads'
 * 2016-02-03: Borrado tipado
 * 2015-03-14: Añadido tipado
 */
function combinar(a, b) {
  return a + b;
}
```

Bien:

```javascript
function combinar(a, b) {
  return a + b;
}
```

**[⬆ Volver arriba](#contenido)**

### Evita los marcadores de secciones

Normalmente acostumbran a ser molestos. Deja que las variables y las funciones
hagan su función con sus identaciones naturales y de esta manera, formateen el
có correctamente
.

Mal:

```javascript
////////////////////////////////////////////////////////////////////////////////
// Instanciación del Modelo Scope
////////////////////////////////////////////////////////////////////////////////
$scope.modelo = {
  menu: "foo",
  nav: "bar"
};

////////////////////////////////////////////////////////////////////////////////
// Preparación de la acción
////////////////////////////////////////////////////////////////////////////////
const acciones = function() {
  // ...
};
```

Bien:

```javascript
$scope.modelo = {
  menu: "foo",
  nav: "bar"
};

const acciones = function() {
  // ...
};
```
