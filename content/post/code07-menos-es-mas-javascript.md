+++
title = "Code 07 - Menos es Mas en JavaScript"
tags = [
	"code",
	"work",
	"git",
	"javascript",
	"Math",
	"ecmascript",
]
date = "2020-10-31"
menu = "main"
description = "Breve resena de un"
+++

Mientras ayudaba a un amigo con un issue en uno de sus proyectos nos topamos con una de esas cosas que hace que a la gente que no le gusta programar en JavaScript le guste menos. En JavaScript existe un objeto llamado __Math__, el cual contiene un conjunto de constantes y funciones bastante útiles para lidiar con operaciones matemáticas.

Dos de estas funciones son __Math.min()__ y __Math.max()__, como sus nombres los indican, retornan el menor y mayor respectivamente dentro de 2 o más números, es decir que hace lo siguiente:

```bash
> Math.min(4, 5, 6, 2, 5, 10, 3);
2
> Math.max(3, 3, 6, 32, 67, 2124, 9);
2124
```

## Cuál es el problema entonces?
Sin googlear y solo adivinando, si llamas cualquiera de las dos funciones sin pasar ningún como parametro, cual seria el comportamiento esperado? ¿Esperarias un 0 o un 1? Vamos a intentarlo:

```bash
> Math.min()
Infinity
> Math.max()
-Infinity
```

Interesante, el mínimo valor del vacío es __Infinity__ y el máximo es __-Infinity__. Bajo esa proposición entonces es correcto decir que Math.min > Math.max:

```bash
> Math.min() > Math.max()
true
```

## Por qué?
Ese comportamiento no es muy intuitivo que digamos aunque si forma parte de la documentación oficial de [JavaScript](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf), al igual que sus descripciones en MDN [Math.min](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/min), [Math.max](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/max). Ahora bien, ¿por qué es este el resultado y no algo como 0?

De forma simple la razón es que para saber cual es el mínimo valor de un conjunto (Math.min), están utilizando como valor inicial el valor numérico más alto posible, o sea, Infinito. De ahí en adelante se empieza a ver el conjunto de parámetros para encontrar el menor, el tema es que no hay parámetros que ver, así que el resultado termina siendo el valor más alto posible, __Infinity__.

Lo mismo para Math.max pero al inverso, empiezan por establecer cuál es el valor mínimo posible y a partir de ahí se busca cual es el valor más grande, como no hay ningún valor que comparar el resultado termina siendo __-Infinity__.

## Conclusiones
Como conclusión principal solo me resta decir que tengan cuidado de cómo utilizan Math.min y Math.max ya que los resultados pueden no ser necesariamente los que esperan.