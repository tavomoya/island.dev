+++
title = "Code 03 - Git Workflow"
tags = [
	"code",
	"work",
	"git",
	"workflow",
	"version control",
]
date = "2020-01-31"
menu = "main"
description = "Breve reseña de mi experiencia con git workflow y la importancia de utilizar control de versiones en los proyectos de software."
+++

En el 2013 aprendi un poco sobre los sistemas de manejar versiones, todo ese conocimiento fue superficial por el momento. Ya sabia que existian y que eran muy usados en (prácticamente) todas las empresas de software, pero aun en ese momento no entendía completamente el por que yo, estudiante de Ingenieria en aquel entonces, necesitaria utilizar algun manejador de versiones. 

Un año más tarde consigo mi primer empleo como desarrollador y es aquí cuando me adentro al mundo de Git. Esta empresa ya usaba SVN como manejador de versiones para otros proyectos, el cual utilizaban no solo para versionar el código fuente de las aplicaciones de estos proyectos, sino también para versionar documentos en general (documentos de análisis de requerimientos, pruebas, etc), sin embargo, el equipo en el cual me tocó trabajar había empezado a utilizar Bitbucket.

## Importancia de versionar el código
Muchas personas (incluyendo al yo del 2013) piensan que versionar solamente tiene sentido cuando se trabaja con un gran equipo o si el proyecto es relativamente grande, pero veámoslo de esta forma, imaginemos que estamos construyendo algo simple como una calculadora, empezamos agregando las operaciones aritméticas de suma y resta, luego de agregar la multiplicacion y division nos damos cuenta de que en algún punto mientras hicimos eso rompimos cierta funcionalidad de la suma y la resta, en este caso quizás la aplicación no ha crecido tanto y quizás no sea tan problemático arreglar esto, seguimos agregando operaciones, potenciación, raíces, derivadas e integrales y luego mientras probamos nos topamos de nuevo con que en algún punto mientras agregamos toda esta funcionalidad rompimos la división, nueva vez pudiéramos simplemente reparar el error pero definitivamente seria mas facil saber específicamente qué cambio desencadenó el problema.

En equipos esto se hace incluso más importante, dado a que serían diferentes programadores trabajando sobre un mismo código fuente, lo cual implica que de vez en cuando haya **conflictos** donde el cambio de uno afecte los cambios de otro.

## A lo que vinimos, a ver el workflow
Un _disclaimer_, no existe un workflow “correcto” o “estándar”, cada equipo o empresa determinara cual es el que mejor se adecue a sus necesidades o arquitectura. Dicho esto el workflow presentado en este artículo es uno con el que llevo trabajando ya alrededor de  4 años y ciertamente nos ha resultado mejor que otros que he usado en otros equipos.

![](/git-branches.png)

### Branching
El arma principal de este flujo particular es el de tener los branches o ramas con fines específicos. Saber para qué existe cada branch es esencial. Existen dos branches “principales”:

- master
- dev/developmnt/develop

El branch master **nunca** debe ser tocado, debe estar siempre estable. Todo lo que existe en master esta en produccion. El branch dev (develop/development en otros equipos con quienes trabajaba) es el que usa cada integrante del equipo para trabajar cada asignación, suele estar más adelantado que master ya que ciertos features o fixes ya hayan mezclados aquí.

Aparte de estos dos branches principales tenemos entonces los branches que crean cada uno de los miembros del equipo. Cada branch debe tener un propósito, si la asignación es la de un nueva funcionalidad o de reparar algún problema encontrado. Una buena convención para nombrar branches es la de utilizar el tipo de asignación como prefijo para el nombre del branch:

- feature
- fix
- hotfix

Por lo que el nombre del branch seria _Tipo de asignación_/_asignación_. Otra sugerencia para agregar nombres significativos a los branches es si se utiliza alguna aplicación de manejar proyectos en la que cada tarea tenga un código único (como Jira), se utilice este código como nombre de Branch.

### Commits
Para este workflow los commits deben ser preferiblemente atómicos, es decir, que en vez de esperar terminar toda la asignacion para hacer un solo commit, es preferible dividir la asignación en varias tareas pequeñas a las que se puedan hacer commit por separado. 

Algo a tomar en cuenta con los commits es el mensaje, lo preferible es escribir un mensaje descriptivo de los cambios o adiciones que contiene el commit. 

### Pull Request
Luego de terminar todo nuestro trabajo solo nos queda crear un pull request. La creación del pull request somete a revisión todos los cambios realizados en el branch de la asignación. 

Todos los pull requests se deben hacer hacia el branch dev/develop/development y luego de que se pruebe la funcionalidad y pasen un Code Review pueden ser mezclados. Esta fase de mezclado puede variar en cada equipo dependiendo de si tienen algún tipo de pipeline de CI/CD o si previo a mezclar con dev, se hacen pruebas preliminares de Release Candidate (Los Release Candidates o RC son un tipo de branch en donde se agrupa un conjunto de funcionalidades y fixes con el objetivo de hacer pruebas preliminares antes de realizar un release).

## Nota Final
Como ya mencioné tengo poco mas de anos trabajando bajo este workflow en Git y nos ha resultado super bien en el equipo con una que otra modificación en algunos repositorios según se han necesitado. ¿Usas algún tipo de git workflow en tu equipo?