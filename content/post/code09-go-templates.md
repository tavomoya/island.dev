+++
title = "Code 09 - Go Templates"
tags = [
	"code",
	"work",
	"git",
	"golang",
	"templates",
	"code generation",
	"library",
	"html"
]
date = "2021-02-04"
menu = "main"
description = "Una mirada a los templates en Go y la funcionalidad de generar codigo a partir de estos templates."
+++

Templates, suena a que empezaré a hablar sobre plantillas visuales para realizar algún tipo de aplicación. Quienes vienen de trabajar en JavaScript sin embargo puede que tengan otra idea (muy similar debo decir), ya que JavaScript también tiene [Template Literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals).

Llevo ya unos cuantos años trabajando con Go como mi lenguaje principal y una de las primeras cosas con las que me topé en la empresa para la que laboro es el uso de Templates en Go.

## Qué es un Template?

Los templates son mecanismos de creación de texto a través del uso de data, o sea, que generan texto basado en información que se le provee. Esto hace que los templates sean utilizados para el envío de correos electrónicos, uso de algunos CLI como __kubectl__ y creación de páginas web. De hecho, la página donde estás leyendo esto está creada usando [__Hugo__](https://gohugo.io/), un generador de páginas web estáticas que utiliza la librería de templates de Go.

Hoy sin embargo quiero hablar sobre un uso muy interesante que también tienen los templates, me refiero a generar código que puedes usar normalmente. ¿Cómo así? Bueno, técnicamente si podemos generar texto, podemos generar código. 

Cuando se trata de generación de código, un caso de uso (y es el que usamos en mi trabajo) es el de generar modelos, queries y métodos basados totalmente en información de una base de datos. La idea es que el programa cree automáticamente los modelos de cada una de las tablas y que aparte de esto cree algunos queries que se puedan reusar.

Sin embargo hoy quiero hablar de cómo también pudiera utilizarse para generar código repetitivo como por ejemplo un set de funciones para manejar slices (array) específicos de cada tipo. Verán, Go es un lenguaje tipado y que además de esto no cuenta con Generics, lo que quiere decir que si creo una función que busque un string dentro de un arreglo de strings, cuando necesite una función que haga lo mismo para int, float o algún tipo custom como sería un struct tendría que crear nuevamente la función a mano para cada tipo.

## Vamos al Código

Para el ejemplo en cuestión vamos a crear un programa que genere una función “SliceContains”, la función devolverá true si el valor pasado por parámetro existe en el slice (array). Vamos a generar la función para todos los tipos de datos primitivos y los agregaremos a un archivo llamado “generated_funcs.go”.

Lo primero que haré es definir el template que se usará para generar la función “SliceContains”, para eso el template necesitará el tipo de dato de la función en cuestion:

```golang
func sliceContainsTemplate() string {
	return `
		func SliceContains{{ .CapitalType }}(slice1 []{{.LowerType}}, val {{.LowerType}}) bool {
			for _, v := range slice1 {
				if v == val {
					return true
				}
			}
			return false
		}
	`
}
```

Notar principalmente la sintaxis tipo {{.CapitalType}}, este tipo de sintaxis le dice al template que en esta parte especifica se utilizará el valor de “CapitalType” pasado en la ejecución del template.

Si vamos a la función main de nuestro programa podemos crear un archivo llamado “generated_funcs.go” en el mismo directorio y ejecutar cada uno de los templates, escribiendo el resultado en el archivo.

```golang
	dir, err := filepath.Abs("./")
	if err != nil {
		log.Fatal(err)
	}

	fileName := path.Join(dir, "generated_funcs.go")
	file, err := os.Create(fileName)
	if err != nil {
		log.Fatal(err)
	}

	defer file.Close()
```

Entonces para ejecutar los templates y que realmente puedan escribir código en nuestro archivo utilizamos el paquete “text/template”. Con este paquete podemos definir el string inicial que declaramos como nuestro template a que realmente sea una variable de tipo template y ejecutarlo pasándole todos los valores que así necesitemos.

Dado que en nuestro programa queremos crear funciones para múltiples tipos de datos, cree un slice de strings con los diferentes tipos con los que quiero probar, e iterando sobre ese slice puedo ejecutar mis templates:

```golang
func main() {

	types := []string{
		"string",
		"int",
		"int64",
		"float64",
	}

	var tpl *template.Template
	var headerTpl *template.Template

	dir, err := filepath.Abs("./")
	if err != nil {
		log.Fatal(err)
	}

	fileName := path.Join(dir, "generated_funcs.go")
	file, err := os.Create(fileName)
	if err != nil {
		log.Fatal(err)
	}

	defer file.Close()

	w := bufio.NewWriter(file)

	tpl = template.Must(template.New("sliceContainsTemplate").Parse(sliceContainsTemplate()))
	headerTpl = template.Must(template.New("headerTemplate").Parse(headerTemplate()))

	headerTpl.Execute(w, nil)

	for _, v := range types {
		tpl.Execute(
			w,
			templateData{
				CapitalType: strings.Title(v),
				LowerType:   v,
			},
		)
	}

	w.Flush()
}
```

Antes de mostrar el resultado notar que estoy ejecutando dos templates, uno dentro del for y otro fuera, la razón de esto es porque ese template de fuera del bucle representa código que quiero que se escriba en el archivo solo una vez, no importa la cantidad de elementos que tenga mi arreglo. En este caso particular ese template lo que genera es la declaración del paquete en que se encuentra el archivo, en este caso “package main”.

```golang
func headerTemplate() string {
	return `package main`
}
```

## Ahora si, veamos el resultado

```bash
$ ls
go.mod  main.go  README.md

$ go run main.go

$ ls
generated_funcs.go  go.mod  main.go  README.md
```

```golang
package main

func SliceContainsString(slice1 []string, val string) bool {
	for _, v := range slice1 {
		if v == val {
			return true
		}
	}
	return false
}

func SliceContainsInt(slice1 []int, val int) bool {
	for _, v := range slice1 {
		if v == val {
			return true
		}
	}
	return false
}

func SliceContainsInt64(slice1 []int64, val int64) bool {
	for _, v := range slice1 {
		if v == val {
			return true
		}
	}
	return false
}

func SliceContainsFloat64(slice1 []float64, val float64) bool {
	for _, v := range slice1 {
		if v == val {
			return true
		}
	}
	return false
}
```

Como se puede ver tenemos un nuevo archivo con las funciones correspondientes de los tipos que especificamos en el main.

¿Qué uso le darías a esta funcionalidad de los templates? Aquí dejo algunos otros links relacionados a este paquete de Go. Si te interesa jugar un poco mas con este codigo lo tengo subido en [GitHub](https://github.com/tavomoya/templates-example). 

[Package Text Template](https://golang.org/pkg/text/template/)

[Package HTML Template](https://golang.org/pkg/html/template/)

[Introduction to Templates by Jon Calhoun](https://www.calhoun.io/intro-to-templates/)

[Learn and Use Templates in Go](https://levelup.gitconnected.com/learn-and-use-templates-in-go-aa6146b01a38)
