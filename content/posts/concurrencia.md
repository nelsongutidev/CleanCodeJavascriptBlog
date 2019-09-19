---
title: "Concurrencia"
description: ""
date: 2019-08-12
order: 8
lang: "es"
---

## Concurrencia

### Usa Promesas, no callbacks

_Los callbacks son funciones que se pasan como parámetros a otras funciones
para ser ejecutaras una vez esta función termina. Por ejemplo: Dada las funciones
A y B, se dice que B es el callback de A si la función B es pasada como parámetro
a la función A y esta, se ejecuta este callback una vez ha terminado_

Los `callbacks` no son limpios ni en cuanto a legibilidad ni en cuanto a formato
de texto _(dado que provocan niveles de identación)_. Con ES2015/ES6 las promesas
son un tipo global. ¡Úsalas!

Mal:

```javascript
import { get } from "request";
import { writeFile } from "fs";

get(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  (requestErr, respuesta) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      writeFile("article.html", respuesta.body, writeErr => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log("File written");
        }
      });
    }
  }
);
```

Bien:

```javascript
import { get } from "request";
import { writeFile } from "fs";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(respuesta => {
    return writeFile("article.html", respuesta);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**[⬆ Volver arriba](#contenido)**

### Async/Await is incluso más limpio que las Promesas

Las promesas son una elección más limpia que los callbacks pero ES2017/ES8
trae la funcionalidad de `async/await` que es incluos más limpio que las promesas.
Todo lo que tienes que hacer es añadir el prefijo `async` a una función y entonces
ya podemos usar esa función de manera imperative sin ningún `.then()`. La
palabra `await` la usarás para hacer que ese código asincrono se comporte de
"manera síncrona".

Mal:

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-promise";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(respuesrta => {
    return writeFile("article.html", respuesrta);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

Bien:

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-promise";

async function conseguirArticulosDeCodigoLimpio() {
  try {
    const respuesrta = await get(
      "https://en.wikipedia.org/wiki/Robert_Cecil_Martin"
    );
    await writeFile("article.html", respuesrta);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}
```
