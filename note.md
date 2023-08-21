# Guía básica sobre Git

## 1. ¿Qué es Git?

* Git es un sistema de contro de versiones que maneja sus datos como una secuencia de copias instantáneas.

![Instantaneas en Git][img1]
***

* Git tiene **tres estados** principales:
  * **Confirmado (committed) :** significa que los datos están almacenados de manera segura en tu base de datos local.
  * **Modificado (modified) :** significa que has modificado el archivo pero todavía no lo has confirmado a tu base de datos.
  * **Preparado (staged) :** significa que has marcado un archivo modificado en su versión actual para que vaya en tu próxima confirmación.
  ***
* Todo proyecto en Git tiene **tres secciones** principales:
  * **Directorio de Git (Git directory) :** es donde se almacenan los metadatos y la base de datos de objetos para tu proyecto. Es la parte más importante de Git, y es lo que se copia cuando clonas un repositorio desde otra computadora.
  * **Directorio de trabajo (working directory) :** es una copia de una versión del proyecto. Estos archivos se sacan de la base de datos comprimida en el directorio de Git, y se colocan en disco para que los puedas usar o modificar.
  * **Area de preparación (staging area) :** es un archivo, generalmente contenido en tu directorio de Git, que almacena información acerca de lo que va a ir en tu próxima confirmación. A veces se le denomina índice (“index”), pero se está convirtiendo en estándar el referirse a ella como el área de preparación.

***

* El **Flujo de trabajo** es algo así:
![areas][img2]

  1. Modificas una serie de archivos en tu **working directory**. Si ha sufrido cambios desde que se obtuvo del repositorio, pero no se ha preparado, **está modificada (modified)**.
  2. Preparas los archivos, añadiéndolos a tu **staging area**. Si ha sufrido cambios desde que se obtuvo del repositorio, pero ha sido añadida al área de preparación, **está preparada (staged)**.
  3. Confirmas los cambios, lo que toma los archivos tal y como están en el área de preparación y almacena esa copia instantánea de manera permanente en tu **Git directory**. Si una versión concreta de un archivo está en el directorio de Git, **se considera confirmada (committed)**.

## 2. Configurando Git por primera vez

Git trae una herramienta llamada ***git config***, que te permite obtener y establecer variables de configuración que controlan el aspecto y funcionamiento de Git.

```terminal
# Para ver la ruta donde se almacenan estas variables
$ git config --list --show-origin 
```

```terminal
# Para establecer tu identidad
$ git config --global user.name "John Doe"
$ git config --global user.email "johndoe@gmail.com"
```

```terminal
# Para cambiar el editor ej: por Emacs
$ git config --global core.editor emacs
```

```terminal
# Para comprobar la configuración 
$ git config --list
```

```terminal
# Para comprobar una configuración específica git config <key>
$ git config user.name
```

## 3. Obteniendo un repositorio Git

Puedes obtener un proyecto Git de dos maneras. La primera es tomar un proyecto o directorio existente e importarlo en Git. La segunda es clonar un repositorio existente en Git desde otro servidor.

```terminal
# Inicializando un repositorio en un directorio existente
$ git init

# comenzar el seguimiento de esos archivos
$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version'
```

```terminal
# Clonando un repositorio existente. git clone [url]
$ git clone https://github.com/libgit2/libgit2

# Clonando un repositorio existente en otro directorio. git clone [url] [new directory]
$ git clone https://github.com/libgit2/libgit2 mylibgit
```

> Se puede clonar desde distintos protocoles *https://*, *git://*, o por ssh *usuario@servidor:ruta/del/repositorio.git*. [más información.](https://git-scm.com/book/es/v2/Git-en-el-Servidor-Configurando-Git-en-un-servidor#r_git_on_the_server)

## 4. Guardando cambios en el Repositorio

Ya tienes un repositorio Git y un checkout o copia de trabajo de los archivos de dicho proyecto. El siguiente paso es realizar algunos cambios y confirmar instantáneas de esos cambios en el repositorio cada vez que el proyecto alcance un estado que quieras conservar.

Cada archivo de tu repositorio puede tener dos estados:

* Rastreados (tracked) : son todos aquellos archivos que estaban en la última instantánea del proyecto; pueden ser archivos sin modificar, modificados o preparados.
* Sin rastrear (untracked): son todos los demás - cualquier otro archivo en tu directorio de trabajo que no estaba en tu última instantánea y que no está en el área de preparación **staging area**. En otras palabras Git ve archivos que no tenías en el *commit* anterior y no los incluirá en el próximo *commit* a menos que se le indique explícitamente.

> Para rastrear los archivos se usa el comando *git status* y en su forma abreviada *git status -s* o *git status --short*

![lifecycle][img3]

### Ejemplo lifecycle

En el siguiente enlace se explican las diferentes situaciones:

* [Ejemplo 1](./ejemplo_1/)

***
> Los archivos clonados por primera vez son **Tracked and unmodified**.
***
> Mientras editas archivos son **modified**, pues han sido cambiados desde su último commit. Luego preparas estos archivos modificados y finalmente confirmas todos los cambios preparados, y repites el ciclo.
***




>
git status


git rm
git rm --cached
***
[img1]: ./img/snapshots.png "img 1. Almacenamiento de datos como instantáneas del proyecto a través del tiempo"
[img2]: ./img/areas.png "img 2. Flujo de trabajo en Git"
[img3]: ./img/lifecycle.png "img 3. Ciclo de vida del estado de los archivos"
