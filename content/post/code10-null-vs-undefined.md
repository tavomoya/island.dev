+++
title = "Code 10 - Null vs Undefined en JavaScript"
tags = [
	"code",
	"work",
	"git",
	"javascript",
	"null",
	"undefined",
	"falsy values",
	"js"
]
date = "2021-08-12"
menu = "main"
description = "Explicando las diferencias y similitudes entre null y undefined en JavaScript."
+++

![](/null-undefined.png)

Null y Undefined, si has trabajado con JavaScript es casi seguro que te hayas topado con uno de estos dos conceptos o muy probablemente con ambos. Pero, ¿por qué existen los dos? ¿Algo que sea null no lo hace undefined y viceversa?

Lo primero que haremos es definir, según el lenguaje, para que se usa cada cosa:

- undefined: algo que esta declarado pero no está definido
- null: algo que esta vacío o cuyo valor es inexistente

```javascript
let x;

console.log(x); 
// undefined
```

```javascript
let y = null;

console.log(y); 
// null
```

Otra diferencia recae en el **tipo** de cada uno, undefined es, por sí solo, un tipo en javaScript, mientras que null es un objeto, como se muestra a continuación:

```javascript
let x;
let y = null;

console.log(typeof x); 
// undefined

console.log(typeof y); 
// object
```

Aun con estas diferencias, hay otras cosas que hacen que estos dos sean bastante similares, por ejemplo, si tratas de acceder a una propiedad de una variable que este null o undefined te encontrarás con un error.

Otras similitudes son que ambos null y undefined son falsy values, valores que se consideran falsos cuando son evaluados en un contexto booleano, por ejemplo en un if, y que ambos son valores primitivos de JavaScript.

## Entonces cual uso?

¿Honestamente? La que desees, sin embargo, para tener cierta consistencia, usa null cuando quieras asignar una variable vacía y no utilices undefined para asignarlo a una variable. ¿Por qué? Pues porque básicamente JavaScript por sí solo no le pondrá undefined a nada, por lo que poner null a las cosas que podemos controlar parece ser el curso de acción más apropiado.