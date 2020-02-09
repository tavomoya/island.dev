+++
title = "Code 04 - Code Review"
tags = [
	"code",
	"work",
	"git",
	"workflow",
	"version control",
	"code review",
	"pull request",
]
date = "2020-02-09"
menu = "main"
+++

En el artículo anterior sobre [Git Workflow](https://tavomoya.dev/post/code03-git-workflow/) hice mención sobre Code Reviews y quise ampliar un poco más sobre mi experiencia recibiendo feedback de reviewers, así también como de mi mismo siendo reviewer.

Los Code Reviews son una práctica que ha adoptado la industria con el propósito de mejorar la calidad del código de nuestros proyectos. Al incluir uno o varios colaboradores que se presten para hacer revisión de cada uno de los pull requests (PRs) se incentiva a una cultura colaborativa al mismo tiempo de que todos los involucrados en el proceso pueden aprender o enseñar algo.

## Desde el punto de vista de quien escribe el código

Mirándolo desde el punto de vista de alguien que recibe Code Reviews desde los inicios de mi carrera como desarrollador mis recomendaciones para los reviewers son las siguientes:

- **Revisión profunda del código:** Con esto me refiero a que la revisión no debería limitarse a solo errores de sintaxis o falta de comentarios en una función, sino que, aparte de los ya mencionados, se analice qué tan bueno es el rendimiento del código o si la dirección tomada en la solución es la correcta tomando en cuenta otras partes del código fuente.

- **Responder o revisar el código “a tiempo”:** Creo que es una de las cosas en las que incluso yo como uno de los reviewers de mi equipo debo revisar. No creo que el reviewer debe empezar a revisar el código inmediatamente se crea el Pull Request, sin embargo, entiendo que dejar demasiado tiempo un Pull Request sin revisar puede confundir luego, el autor del código puede que no tenga tan fresco en la mente alguna de las razones por la cual programo algo de cierta manera, o incluso si ya se ha hecho algun release antes de revisar este feature que haga que sea inservible de la manera en la que está escrito.

- **Retroalimentación:** Esta es una de las cosas que ayuda muchísimo cuando aún se es un Junior, ya que puede que resolvamos problemas pero quizás no de la manera más “limpia” o “eficiente”. Una buena retroalimentación debe estar basada en una crítica constructiva a lo que ha escrito el autor del código, pero al mismo tiempo instruyendo cual debería ser una mejor solución al caso en cuestión.

- **Empatía:** Igual que en la anterior, una de las que más me ayudó cuando era un Junior, es que el reviewer empatice conmigo. Lo que quiero decir es que entienda por que hago las cosas de la forma que las hago, quizás soy nuevo en la empresa/proyecto y aun no tengo todo el conocimiento de cómo funciona el proyecto, quizás soy un Junior que aún está en etapa de aprendizaje de las tecnologías utilizadas en el proyecto.

Aparte de estos puntos creo que otra cosa importante como autor de código debe ser el aprender de toda la retroalimentación y revisiones que se hagan del código. Sirve como aprendizaje de la tecnología en la que se trabaje y del proyecto en el que se está trabajando también. 

## Desde el punto de vista de quien revisa el código

Por otro lado, cuando lo veo desde el punto de vista del reviewer, estas son las recomendaciones que doy a quienes escriben código:

- **Mensajes de los commits:** Los mensajes de los commits deben ser lo menos ambiguo posibles pero al mismo tiempo deben tener una buena descripción de lo que el autor hizo en el commit. Hay que tratar de evitar mensajes como “fixed stuff” “refactor component”, estos mensajes no describen absolutamente nada de lo que el autor escribió y hacen que el review sea más tedioso de la cuenta.

- **Evitar commits gigantes:** Otra cosa relacionada a los commits es el tamaño de los cambios por cada commit. Cuando se está trabajando en una nueva funcionalidad o arreglando algún bug, es buena práctica desglosar la tarea en cuestión en tareas más pequeñas que se pueden ir resolviendo lentamente y haciendo un commit por cada una de estas. 

- **Aprender de la retroalimentación:** En el otro punto de vista mencionaba lo importante de que el reviewer pueda dar un retroalimentación a el autor del código, pero también ocurre que el autor puede sentirse mal por alguna indicación que se le haya dado en una retroalimentación. Estas críticas nunca son con el motivo de hacer sentir mal al autor, para nada, nuestro trabajo consiste en que el código se mantenga lo más limpio y eficiente posible. 

## Sobre lo que hay que revisar

### El uso correcto de estándares

En cada equipo se designan estándares que se utilizan en toda la aplicación, la forma de nombrar variables, funciones o clases, el uso de ciertas librerías o la forma de estructurar el código en sí. 

Procurar que el autor del código haya seguido con todos los estándares predefinidos en el equipo es una de las cosas principales a las que un reviewer debe prestar atención, ya que no se debe perder la consistencia que existe en el código fuente de tu proyecto.

### Potential Rework

Lo de Potential Rework es una filosofía en la que como reviewer veo el codigo del autor como un adversario contra el cual debo tratar de buscar alguna falla o potencial problema en el código. No se trata de molestar al autor tratando de buscar cualquier problema, sino de encontrar algún caso específico que no haya sido tomado en cuenta en la realización de la tarea.

### Cosas que NO hay que revisar

Soy creyente de que hay cosas que no deben ser revisadas, por ejemplo cosas como la indentación no debe ser algo a lo que el reviewer debe prestarle mucha atención. Creo que esto forma parte esencial de la consistencia del código, pero creo que mejor encargarle la responsabilidad del formateo del código a alguna herramienta es mejor que andar revisando cuantos tabs o espacios está usando el autor del código. 

El uso de herramientas y la normalización de estas en un equipo es esencial, existen muchos linters y formatters para prácticamente todos los lenguajes de programación, por lo que no tiene sentido invertir el tiempo revisando estos. 

