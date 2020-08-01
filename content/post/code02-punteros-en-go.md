+++
title = "Code 02 - Punteros en Go"
tags = [
	"code",
	"work",
	"go",
	"golang",
	"pointers",
]
date = "2019-10-23"
menu = "main"
description = "Tutorial de como funcionan los punteros en Golang"
+++

Go es uno de mis lenguajes de programación favoritos, llevo trabajando con él desde finales de 2016. Venía de trabajar ya 2 años con Nodejs como mi lenguaje principal en el backend por lo que muchas cosas de Go me tomaron de sorpresa al principio. Lo primero que note al empezar a estudiar Go fue el uso de punteros. No veía punteros desde que estaba en la universidad aprendiendo C, por lo que se podrían imaginar lo complicado que me resultó refrescar mis conocimientos sobre estos.  

Los punteros son utilizados para comunicar data en una aplicación haciendo referencia a la dirección en memoria de dicha data, en vez de pasar el contenido de la misma. Viéndolo de manera práctica, utilizariamos punteros si queremos que la data original sea mutada por la función que la recibe en vez de que la función haga una copia de esta data para funcionar. 

## Sintáxis
Para definir o usar punteros en Go se utilizan algunos elementos, el primero es el **ampersand (&)**, el cual indica que la variable que precede será un puntero. O sea:
```go
var str string = "hola"
var strPointer *string = &str // strPointer es un puntero a str
```

El segundo elemento que se utiliza (y que utilizo en el ejemplo de arriba), es el **asterisco (*)**, este tiene dos usos, el primero es cuando declaramos variables donde el tipo tiene como prefijo el asterisco denota que la variable es un puntero del tipo especificado. El segundo uso es para hacer _dereferencing_ o sacar el valor del puntero (sacar el contenido de la variable en cuestión)
```go
var str string = "hola"
var strPointer *string = &str // strPointer es un puntero a str

fmt.Println("str string = ", str)
fmt.Println("strPointer *string = ", strPointer) // No muestra el valor, sino la dirección en memoria
fmt.Println("strPointer value = ", *strPointer) // Uso del asterisco como prefijo para sacar el valor del puntero
```

**Output**
```bash
$ go run main.go 
str string =  hola
strPointer *string =  0xc04204c1c0
strPointer value =  hola
```

Para reflejar un poco el comportamiento que explicaba al principio vamos a ver qué pasa con la variable **str** cuando modificamos el puntero **strPointer**:
```go
var str string = "hola"
var strPointer *string = &str // strPointer es un puntero a str

fmt.Println("str string = ", str)
fmt.Println("strPointer *string = ", strPointer) // No muestra el valor, sino la dirección en memoria
fmt.Println("strPointer value = ", *strPointer) // Uso del asterisco como prefijo para sacar el valor del puntero

*strPointer = "adios"
fmt.Println("strPointer value = ", *strPointer)
fmt.Println("str string = ", str)
```

**Output**
```bash
$ go run main.go 
str string =  hola
strPointer *string =  0xc04204c1c0
strPointer value =  hola
strPointer value =  adios
str string =  adios
```

Vemos que sin tener que explícitamente cambiar el valor de la variable **str**, este es cambiado utilizando también el puntero a la dirección de memoria donde está alojado.

## Punteros en funciones
En Go es más común ver punteros usados para definir argumentos o el tipo del valor que retorna. Las funciones pueden recibir sus argumentos de dos formas:

* Por valor: Donde una copia del valor es pasado a la función, lo que significa que cualquier actualización que reciba solo afectará la variable dentro de la función.
* Por referencia: Donde se pasa un puntero a la variable, en este caso los cambios que se hagan dentro de la función, también afectan la variable original. 

Decidir cual usar va a depender meramente del caso de uso y de si necesitas o no que la variable original sea modificada por una función. Así que, si no necesitas que la variable cambie, pasa el argumento por valor, en caso contrario, usa punteros.

**Usando un argumento pasado por valor**

```go
package main

import "fmt"

type Person struct {
	Name string
}

func main() {
	var person Person = Person{Name: "Greg"}

	fmt.Println("struct original: ", person)
	updatePersonName(person)
	fmt.Println("struct no cambiado: ", person)
}

func updatePersonName(person Person) {
	person.Name = "Gus"
	fmt.Println("Dentro de updatePersonName: ", person)
}
```
**Output**
```bash
$ go run main.go 
struct original:  {Greg}
Dentro de updatePersonName:  {Gus}
struct no cambiado:  {Greg} 
```

**Usando un argumento pasado como puntero**
```go
package main

import "fmt"

type Person struct {
	Name string
}

func main() {
	var person Person = Person{Name: "Greg"}

	fmt.Println("struct original: ", person)
	updatePersonName(&person)
	fmt.Println("struct cambio: ", person)
}

func updatePersonName(person *Person) {
	person.Name = "Gus"
	fmt.Println("Dentro de updatePersonName: ", *person)
}
```
**Output**
```bash
$ go run main.go 
struct original:  {Greg}
Dentro de updatePersonName:  {Gus}
struct cambio:  {Gus} 
```

## Punteros como _receivers_ de Métodos
Los métodos en Go son funciones que están ligadas a un tipo. Go no tiene clases, por lo que esta es la forma correcta de agregar funcionalidad a un tipo. La forma en la que se declaran es utilizando algo a lo que le llaman receiver, el cual es un set de argumentos definidos entre el keyword **func** y el nombre de la función. 

Al igual que las funciones, los métodos tienen comportamientos diferentes dependiendo de si el receiver es un puntero o no, si es un puntero pues lo mismo, el valor del receiver puede ser mutado en el método, si no pues los cambios serán sólo locales.

**Usando receiver como valor**
```go
package main

import "fmt"

type Person struct {
	Name string
}

func main() {
	var person Person = Person{Name: "Greg"}

	fmt.Println("struct original: ", person)
	person.updatePersonName("Gus")
	fmt.Println("struct no cambio: ", person)
}

func (p Person) updatePersonName(newName string) {
	p.Name = newName
	fmt.Println("Dentro de updatePersonName: ", p)
}
```
**Output**
```bash
$ go run main.go 
struct original:  {Greg}
Dentro de updatePersonName:  {Gus}
struct no cambio:  {Greg}
```

**Usando receiver como puntero**
```go
package main

import "fmt"

type Person struct {
	Name string
}

func main() {
	var person Person = Person{Name: "Greg"}

	fmt.Println("struct original: ", person)
	person.updatePersonName("Gus")
	fmt.Println("struct cambiado: ", person)
}

func (p *Person) updatePersonName(newName string) {
	p.Name = newName
	fmt.Println("Dentro de updatePersonName: ", *p)
}
```

**Output**
```bash
$ go run main.go 
struct original:  {Greg}
Dentro de updatePersonName:  {Gus}
struct cambiado:  {Gus}
```

**Nil Safety**
Los punteros pueden ser bastante útiles dependiendo de la necesidad que tengamos, sin embargo hay un factor clave que hay que tener en cuenta al momento de utilizar punteros y es que estos pueden ser **nil**. Nil en Go es null, todos los tipos tienen un valor cero (valor por defecto que tiene una variable a la que no le hemos asignado ningún valor), el valor cero de los punteros es nil, básicamente representa que la variable no ha sido inicializada con nada.

Como los punteros pueden ser nil, al momento de crear funciones que manejen punteros, es necesario que se confirme antes de hacer cualquier cambio o lectura del valor del puntero si este es nil o no. A esto le llaman **nil safe functions** y basta con solamente hacer una condición que confirme que el puntero no sea nil.

La razón principal por la cual es importante que las funciones que usan punteros sean seguras, es porque cuando se intenta manipular un puntero que es nil puede provocar un **panic**. Un panic es un error que ocurre en tiempo de ejecución, usualmente son errores de programación, cualquier otro tipo de error es manejado usando el tipo error de Go.

Volviendo al ejemplo donde usamos un argumento como puntero, notamos que no es "nil safe":
```go
package main

import "fmt"

type Person struct {
	Name string
}

func main() {
	var person *Person

	fmt.Println("struct original: ", person)
	updatePersonName(person)
	fmt.Println("struct no cambiado: ", person)
}

func updatePersonName(person *Person) {
	person.Name = "Gus"
	fmt.Println("Dentro de updatePersonName: ", *person)
}
```
**Output**
```bash
$ go run main.go 
struct original:  <nil>
panic: runtime error: invalid memory address or nil pointer dereference
[signal 0xc0000005 code=0x1 addr=0x8 pc=0x48b5ed]
```

**Haciendo que _updatePersonName_ sea "nil safe":**
```go
package main

import "fmt"

type Person struct {
	Name string
}

func main() {
	var person *Person

	fmt.Println("struct original: ", person)
	updatePersonName(person)
	fmt.Println("struct no cambiado: ", person)
}

func updatePersonName(person *Person) {
	if person == nil {
		fmt.Println("Person is nil")
		return
	}

	person.Name = "Gus"
	fmt.Println("Dentro de updatePersonName: ", *person)
}
```

**Output**
```bash
$ go run main.go 
struct original:  <nil>
Person is nil
struct no cambiado:  <nil>
```

## Conclusión
Saber cuándo definir funciones y/o métodos con argumentos pasados por valor o referencia es de vital importancia al momento de trabajar con Go. Tener control de cuando y donde una variable puede mutar puede ayudarnos a construir mejor software.
