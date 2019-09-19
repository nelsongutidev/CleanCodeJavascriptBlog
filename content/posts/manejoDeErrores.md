---
title: "Manejo de Errores"
description: ""
date: 2019-08-12
order: 9
lang: "es"
---

## Manejo de errores

¡Lanzar errores está bien! Significa que en tiempo de ejecución se ha detectado
que algo no estaba funcionando como debía y se ha parado la ejecución del trozo
de código. Además se notifica siempre en la consola del navegador.

### No ignores los errores capturados

No hacer nada con los errores capturados no te da la opción de anticiparte
o arreglar dicho error. El printar el error por la consola del navegador
no es una solución, pues la gran mayoría de veces nadie es consciente de eso
y el error pasas desapercibido. Envuelve tu código con `try/catch` y es ahí
donde tendrás que elaborar tu plan de reacción a posibles errores

Mal:

```javascript
try {
  functionQueDeberiaLanzarError();
} catch (error) {
  console.log(error);
}
```

Bien:

```javascript
try {
  functionQueDeberiaLanzarError();
} catch (error) {
  // Una option (algo más molesta que el convencional console.log)
  console.error(error);
  // Otra opción:
  notificarAlUsuarioDelError(error);
  // Otra opción:
  reportarElArrorAUnServicio(error);
  // O hazlas todas!
}
```

### No ignores las promesas rechazadas

No ignores las promesas que han sido rechadas por la misma razón que no deberías
ignorar errores capturados en el `try/catch`.

Mal:

```javascript
cogerDatos()
  .then(datos => {
    functionQueDeberiaLanzarError(datos);
  })
  .catch(error => {
    console.log(error);
  });
```

Bien:

```javascript
cogerDatos()
  .then(datos => {
    functionQueDeberiaLanzarError(datos);
  })
  .catch(error => {
    // Una option (algo más molesta que el convencional console.log)
    console.error(error);
    // Otra opción:
    notificarAlUsuarioDelError(error);
    // Otra opción:
    reportarElArrorAUnServicio(error);
    // O hazlas todas!
  });
```
