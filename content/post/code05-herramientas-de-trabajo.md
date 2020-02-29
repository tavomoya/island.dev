+++
title = "Code 05 - Herramientas de Trabajo"
tags = [
	"code",
	"work",
	"git",
	"vscode",
	"eslint",
	"gitlens",
	"rest client",
]
date = "2020-02-29"
menu = "main"
+++

Uno de los temas sugeridos por algunas personas desde ya hace un tiempo es el de hablar sobre las herramientas de trabajo que uso en el dia a dia. Esta no es una lista de “recomendaciones”, sino una representación de herramientas que me han funcionado durante cierto tiempo.

## Task Trackers

![](/trello.png)

Para dar seguimiento a las tareas en las que estoy trabajando uso dos aplicaciones principales. En mi empleo actual utilizamos Jira para el manejo del proyecto y sus tareas, sin embargo en mis proyectos personales y alguno que otro proyecto freelance utilizo Trello.

Trello es una de mis herramientas preferidas por lo fácil de usar además de que me permite utilizarlo en otras cosas que no necesariamente tienen que ver con desarrollo de software. Utilizo Trello incluso para cosas como compras para la casa o para dividir el trabajo de alguna tarea de mi empleo actual.

Ir a Jira: https://www.atlassian.com/software/jira

Ir a Trello: https://trello.com/

## Control de Versiones

Al igual que con el manejo de tareas, utilizo uno para mi empleo y otro para mis asuntos personales/freelance. En mi trabajo utilizamos Bitbucket (el cual tiene una muy buena integración con Jira, ya que son productos de Atlassian), y para mis proyectos personales utilizo GitHub. En ambos tengo repositorios privados, principalmente en Bitbucket ya que el proyecto de la empresa es confidencial. 

No tengo una razón específica por la cual uso GitHub por encima de algún otro como Bitbucket o Gitlab. Cree mi cuenta en GitHub principalmente porque la mayoría de herramientas open source que utilizo tenían el código alojado allí y me pareció un buen lugar en donde compartir mi codigo tambien.

Ir a Bitbucket: https://bitbucket.org/

Ir a GitHub: https://github.com/

## Editor de Texto (y plugins)

Mi editor de texto de preferencia es VS Code, solía usar Sublime Text, de hecho duré 2 años utilizandolo, use Atom por un tiempo, pero mi favorito desde su lanzamiento ha sido VS Code.

![](/vscode.png)

Obtener VS Code: https://code.visualstudio.com/

Aun con las funcionalidades que me trae VS Code por defecto utilizo algunos plugins dependiendo del lenguaje en el que estoy trabajando o de la tarea que esté realizando. Aquí una lista de los que actualmente uso:

### Git Lens

![](/gitlens.png)

Lo utilizo principalmente para hacer code reviews, pero además de esto mientras estoy desarrollando este plugin me permite ver la información del último commit de la línea en la que estoy, lo cual puede ser muy bueno de que necesite ver algún tipo de información del historial del archivo o función que esté modificando.

Obtener GitLens: https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens

### Bracket Pair Colorizer 2

![](/bc2.JPG)

Este se explica solo por el nombre, la función de este plugin es simplemente colorear del mismo color cada par de brackets, lo cual ayuda cuando se está trabajando con funciones, condiciones, objetos o cualquier cosa en la que se deba usar brackets.

Obtener Bracket Pair Colorizer 2: https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer-2

### ESLint
Uno bastante conocido, es un linter para trabajar con JavaScript y TypeScript. Básicamente un linter es una herramienta que analiza el código en busca de algún tipo de problema y también de que se siga el mismo estilo.

Obtener ESLint: https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint

### vscode-go
Es prácticamente también un linter pero para Go, aparte tiene funcionalidades como autoformateo del código y agrega los imports (y remueve los que no se usan) cada vez que se guarda.

Obtener vscode-go: https://marketplace.visualstudio.com/items?itemName=ms-vscode.Go

### Markdown Preview Enhanced
El nombre de este plugin también define su funcionalidad principal, utilizo este para tener una vista previa de los archivos tipo markdown (.md).

Obtener Markdown Preview Enhanced: https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced

## Misceláneos

### Insomnia

![](/insomnia.png)

Es un REST client open source disponible para windows, Linux y MacOS, es bastante bueno y tiene una interfaz bastante limpia. Solía utilizar Postman como REST client y la única razón por la cual cambie es porque me gusta más la experiencia usando Insomnia que usando Postman.

Ir a Insomnia: https://insomnia.rest/

### Vagrant

Es una herramienta que permite administrar ambientes de desarrollo los cuales se pueden utilizar en máquinas virtuales. Vengo usando vagrant en mi empleo actual desde finales de 2016.

Ir a Vagrant: https://www.vagrantup.com/

### Cmder

![](/cmder.png)

Es un emulador de consola para Windows. Trabajo la mitad del tiempo en Linux (Debian 9) y la otra mitad en Windows, por lo que usar la consola por defecto o el PowerShell luego de usar una terminal de Linux es horrible. Cmder me permite utilizar comandos de Linux en windows, además de otras pequeñas funcionalidades.

Ir a Cmder: https://cmder.net/

## Opiniones
¿Qué te parecen estas herramientas, recomiendas alguna otra que deba integrar?
