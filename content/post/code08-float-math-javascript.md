+++
title = "Code 08 - Matemáticas Flotantes en JavaScript"
tags = [
	"code",
	"work",
	"git",
	"javascript",
	"Math",
	"ecmascript",
	"floats"
]
date = "2020-11-08"
menu = "main"
description = "Explicacion de como funcionan las operaciones matematicas en JavaScript cuando utilizamos numeros decimales con precision arbitraria."
+++

Siguiendo la línea del artículo pasado [Code 07 - Menos es Más en JavaScript](https://tavomoya.dev/post/code07-menos-es-mas-javascript/) sobre una de las peculiaridades de JavaScript y el objeto Math, se me ocurrió escribir sobre otra cosa bien específica con la que todos los que hemos trabajado con JavaScript nos hemos topado, y si no, pues estamos en camino a eso.

El problema de hoy es el de las operaciones matemáticas con números flotantes o de precisión arbitraria. Básicamente si tomamos los números __0.1__ y __0.2__, asumirías que la suma daría como resultado __0.3__, sin embargo esto no es lo que pasa en JavaScript:

```bash 
> var a = 0.1;
undefined
> var b = 0.2;
undefined
> a + b;
0.30000000000000004
```

Eso no parece correcto, ¿qué está haciendo JavaScript? Probablemente te estés preguntando, en este caso no es asunto de JavaScript sino de cómo las computadoras funcionan. 

Las computadoras no utilizan un sistema decimal para realizar cálculos, sino que usan uno binario, no solo eso sino que los números siempre son almacenados como enteros por lo que al momento de utilizar números decimales la computadora hace todo lo posible por representarlo de la misma manera en la que uno esperaría en un sistema decimal.

## Punto de Vista Matemático

![](/floats.png)

En un sistema numérico de base 10 como el que usamos los humanos, tenemos formas de representar decimales de forma exacta siempre y cuando la base de la fracción que forma el decimal use uno de los factores primos de 10, como los factores primos de 10 son el 2 y el 5, eso quiere decir que podemos representar un decimal de forma exacta siempre y cuando su fracción sea de tipo ½, ¼, ⅕, ⅛, 1/10, del mismo modo las fracciones cuya base sea 3, 6 o 7 siempre resultará en un número irracional.

Esto se complica aún más cuando lo llevamos a un sistema de base 2 ya que el factor primo es solamente 2, por lo que los únicos decimales que pueden ser exactamente representados son aquellos cuya fracción sea de tipo ½, lo que significa que __0.1__ (que seria 1/10) y __0.2__ (que seria ⅕), son representados como decimales periódicos en sistema binario.

## Volviendo al problema inicial

Un problema de cálculo puede definitivamente afectarte si estás trabajando alguna aplicación financiera (como fue mi caso alrededor de 2014/2015), alguna aplicación estadística, o cualquier otro tipo de proyecto en el que se necesite de un cálculo exacto. 

Ahora bien, este problema como hemos podido ver no es exclusivo de JavaScript, sino básicamente de cómo funcionan las computadoras y de cómo estas realizan cálculos matemáticos cuando utilizamos números decimales. 

## Soluciones

Bien, ya entendemos cual es la problemática, pero cómo podemos solucionar este problema y por qué no lo he visto fuera de JavaScript? Lo cierto es que si ocurre fuera de JavaScript, cada lenguaje tiene su solución para este problema, no existe actualmente un estándar de cómo exactamente realizar operaciones con números flotantes. 

En JavaScript existen varias librerías para lidiar con estos como [bignumber.js](https://github.com/MikeMcl/bignumber.js), [decimal.js](https://github.com/MikeMcl/decimal.js), [accounting.js](https://github.com/openexchangerates/accounting.js), [big.js](https://github.com/MikeMcl/big.js), [js-big-decimal](https://github.com/royNiladri/js-big-decimal) así como me imagino que muchas otras más.

En otros lenguajes como C#, Java, Python y PHP existen tipos, clases y/o paquetes Decimal los cuales permiten realizar operaciones matemáticas precisas utilizando números decimales. 

## Conclusión

Si el redondear números decimales o realizar cálculos matemáticos precisos usando números decimales o realizar comparaciones con números decimales no son la prioridad para el tipo de proyecto en el que estás trabajando quizás esto no sea un problema, sin embargo si estás trabajando cualquier tipo de proyecto ligado a dinero o al cálculo de probabilidades basado en estadísticas, pues te recomiendo que hagas uso de algunas de las librerías mencionadas más arriba.

Aquí dejo otros links que pueden también ser de interés para conocer un poco más del tema, así también como ver de primera mano cómo funciona en (casi) todos los lenguajes de programación. 

[0.30000000000000004](https://0.30000000000000004.com/)

[IEEE Standard for Floating-Point Arithmetic](https://standards.ieee.org/standard/754-2008.html)

[Floating Point Guide](https://floating-point-gui.de/)
