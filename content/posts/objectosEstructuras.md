---
title: "Objectos y estructuras de datos"
description: "Buenas Practicas con Objectos y Estructuras de Datos"
date: 2019-08-12
order: 4
lang: "es"
---

## Objectos y estructuras de datos

### Utiliza setters y getters

Usar `getters` y `setters` para acceder a la información del objeto está mejor
que simplemente accediendo a esa propiedad del objeto. ¿Por qué?

- Si quieres modificar una propiedad de un objeto, no tienes que ir mirando
  si existe o no existe para seguir mirando a niveles más profundos del objeto.
- Encapsula la representación interna (en caso de tener que comprobar cosas, mirar en varios sitios...)
- Es sencillo añadir mensajes y manejos de error cuando hacemos `get` y `set`
- Te permite poder hacer lazy load en caso de que los datos se recojan de una Base de Datos (bbdd)

Mal:

```javascript
function crearCuentaBancaria() {
  // ...

  return {
    balance: 0
    // ...
  };
}

const cuenta = crearCuentaBancaria();
cuenta.balance = 100;
```

Bien:

```javascript
function crearCuentaBancaria() {
  // Esta es privada
  let balance = 0;

  // Un "getter", hecho público a través del objeto que retornamos abajo
  function cogerBalance() {
    return balance;
  }

  // Un "setter", hecho público a través del objeto que retornamos abajo
  function introducirBalance(cantidad) {
    // ... validamos antes de hcaer un balance
    balance = cantidad;
  }

  return {
    // ...
    cogerBalance,
    introducirBalance
  };
}

const cuenta = crearCuentaBancaria();
cuenta.introducirBalance(100);
```

### Hacer que los objetos tengan atributos/métodos privados

Esto se puede hacer mediante `clojures` _(de ES5 en adelante)_.

Mal:

```javascript
const Empleado = function(nombre) {
  this.nombre = nombre;
};

Empleado.prototype.cogerNombre = function cogerNombre() {
  return this.nombre;
};

const empleado = new Empleado("John Doe");
console.log(`Nombre del empleado: ${empleado.cogerNombre()}`); // Nombre del empleado: John Doe
delete empleado.nombre;
console.log(`Nombre del empleado: ${empleado.cogerNombre()}`); // Nombre del empleado: undefined
```

Bien:

```javascript
function crearEmpleado(name) {
  return {
    cogerNombre() {
      return name;
    }
  };
}

const empleado = crearEmpleado("John Doe");
console.log(`Nombre del empleado: ${empleado.cogerNombre()}`); // Nombre del empleado: John Doe
delete empleado.name;
console.log(`Nombre del empleado: ${empleado.cogerNombre()}`); // Nombre del empleado: John Doe
```
