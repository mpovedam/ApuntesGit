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

Para ver los Cambios Preparados y No Preparados
*git diff* te muestra las líneas exactas que fueron añadidas y eliminadas.

* **git diff :** Este comando compara lo que tienes en tu directorio de trabajo con lo que está en el área de preparación. El resultado te indica los cambios que has hecho pero que aun no has preparado. Es decir, - solo verás los cambios que aun no están preparados
* **git diff --staged :** Este comando compara tus cambios preparados con la última instantánea confirmada. Es decir, si quieres ver lo que has preparado y será incluido en la próxima confirmación.
* **git diff \<commit1> \<commit2> :** Compara los cambios entre dos commit. Para obtener los ID de los commits usamos **git log**.
* También se puede usar **git show** para ver cambios que han existido sobre los dos últimos commits.

Uniendo los conceptos vistos anteriormente se puede resumir en la siguiente imagen.
![estados git][img4]

Comparto un enlace donde se puede estudiar los eventos que se presentan en GIT
[git-cheatsheet](https://ndpsoftware.com/git-cheatsheet.html#loc=index;)

Resumen:
![stash][img5]

* **stash :** un lugar para ocultar cambios mientras trabajas en otra cosa.
* **git stash push [\<msg>] :** Guarda las modificaciones locales en un nuevo stash, y luego ejecuta ***git reset ‑‑hard*** para revertirlas. El msg es optativo y agrega una descripción adicional al estado. Para realizar una captura rápida, se pueden omitir tanto "push" como msg.
* **git stash pop :** Aplica los cambios del stash especificado y luego lo elimina de los temporales. Por defecto aplica el último stash.
* **git stash apply [\<stash>] :** Aplica los cambios del stash especificado en el espacio de trabajo. Por defecto aplica el último stash.
* **git stash list :** Lista los stashes disponibles actualmente.
* **git stash show [\<stash>] :** Muestra los cambios almacenados en el stash aplicando un diff entre ese stash y su padre original. Por defecto utiliza el último stash.
* **git stash drop [\<stash>] :** Elimina una entrada del listado de stash. Por defecto elimina el último stash.
* **git stash clear :** Elimina todos las entradas del stash. IMPORTANTE: estas entradas eliminadas pueden ser irrecuperables luego.
* **git stash branch \<branchname> [\<stash>] :**
Crea y posiciona HEAD en el branchname apuntando al commit del cual el stash fue creado originalmente, aplicando luego los cambios almacenados en el stash al nuevo árbol de trabajo.
Si se realiza exitosamente, y stash es una referencia tipo stash@{revision}, el comando elimina el stash. Cuando no se especifica un stash, aplica el último.
Este comando es útil en los casos en que la rama en la que se ejecutó git stash push ha cambiado demasiado por lo que git stash apply fallaría por conflictos. Al aplicar los cambios sobre el commit que fue HEAD al momento de ejecutar git stash originalmente, se restauran los cambios sin conflictos.

![Workspace][img6]

* **workspace :** Espacio de trabajo: archivos locales posicionados en una rama.
* **git status :** Muestra las localizaciones que tienen diferencias entre el index y el commit HEAD actual, localizaciones que tienen diferencias entre el workspace y el index, y localizaciones en el workspace que no están siendo registradas por git.
* **git diff :** Muestra las diferencias no añadidas al index.
* **git diff \<commit or branch> :** Muestra los cambios que existen en el workspace relativos al commit mencionado. Puede usarse HEAD para comparar contra el último commit, o el nombre de una rama (branch) para comparar contra otra rama.
* **git add \<file... or dir...> :** Añade el contenido actual de archivos nuevos o modificados al index, preparando a la vez ese contenido para ser incluido en el próximo commit. Usar add --interactive para añadir los contenidos del espacio de trabajo al index de manera interactiva.
* **git add -u :** Añade el contenido actual de los archivos modificados (NO NUEVOS) al index. Es similar a lo que hace '***git commit -a***' al prepararse para realizar un commit.
* **git rm \<file(s)...> :** Elimina uno o varios archivos del espacio de trabajo e index.
* **git mv \<file(s)...> :** Mueve uno o varios archivos del espacio de trabajo e index.
* **git commit -a [-m 'msg'] :** Realiza un commit con todos los cambios en los archivos desde el último commit, excluyendo los archivos no registrados (ej: todos los archivos que están listados en index). Elimina archivos en index que fueron eliminados del espacio de trabajo.
* **git checkout \<file(s)... or dir> :** Actualiza el archivo o directorio en el espacio de trabajo. Esto NO cambia de rama.
* **git reset --hard :** Equipara el espacio de trabajo y el index al árbol local. ADVERTENCIA: Se pierden todos los cambios a archivos registrados por git desde el último commit. Usar este comando si una combinación/merge resultó en conflictos y es necesario comenzar de nuevo. Al pasar ORIG_HEAD puede deshacerse el merge más reciente y todos los cambios posteriores.
* **git reset --hard \<remote>/\<branch> :** Equipara el espacio de trabajo y el index con una rama remota. Usar reset ‑‑hard origin/main para descartar todos los commits en la rama local main. Se puede utilizar para comenzar de nuevo desde una combinación/merge fallida.
* **git switch \<branch> :** Cambia de rama actualizando el index y el espacio de trabajo para reflejar la rama especificada, branch, y actualizando la posición de HEAD a branch.
* **git checkout -b \<name of new branch> :** Crea una rama y posiciona el HEAD allí
* **git merge \<commit or branch> :** Combina (merge) los cambios de branch name con los de la rama actual. Usar ***‑‑no-commit*** para dejar los cambios sin realizar un commit.
* **git rebase upstream :** Revierte todos los commits desde que la rama actual se separó del \<upstream>, y luego los vuelve a aplicar uno por uno por sobre los commits del HEAD de \<upstream>.
* **git cherry-pick \<commit> :** Aplica los cambios del commit especificado en la rama actual.
* **git revert \<commit> :** Revierte el commit especificado y realiza un commit con el resultado. Esto requiere que el árbol de trabajo esté limpio (sin modificaciones desde el HEAD commit).
* **git clone \<repo> :** Descarga el repositorio especificado por \<repo> y posiciona el HEAD en la rama main.
* **git pull \<remote> \<refspec> :** Incorpora los cambios desde un repositorio remoto en la rama actual. En su modo por defecto, ***git pull*** es un atajo de ***git fetch*** seguido por ***git merge***  FETCH_HEAD.
* **git clean :** Limpia el árbol de trabajo eliminando de forma recursiva los archivos que no están bajo el control de versionado, comenzando por el directorio actual.
* **git stash push [\<msg>] :** Guarda las modificaciones locales en un nuevo stash, y luego ejecuta git reset ‑‑hard para revertirlas. El msg es optativo y agrega una descripción adicional al estado. Para realizar una captura rápida, se pueden omitir tanto "push" como \<msg>.
* **git stash pop :** Aplica los cambios del stash especificado y luego lo elimina de los temporales. Por defecto aplica el último stash.
* **git stash apply [\<stash>] :** Aplica los cambios del stash especificado en el espacio de trabajo. Por defecto aplica el último stash.

![Index][img7]

* **git status :** Muestra las localizaciones que tienen diferencias entre el index y el commit HEAD actual, localizaciones que tienen diferencias entre el workspace y el index, y localizaciones en el workspace que no están siendo registradas por git.
* **git diff :** Muestra las diferencias no añadidas al index.
* **git add \<file... or dir...> :** Añade el contenido actual de archivos nuevos o modificados al index, preparando a la vez ese contenido para ser incluido en el próximo commit. Usar add --interactive para añadir los contenidos del espacio de trabajo al index de manera interactiva.
* **git add -u :** Añade el contenido actual de los archivos modificados (NO NUEVOS) al index. Es similar a lo que hace '***git commit -a***' al prepararse para realizar un commit.
* **git rm \<file(s)...> :** Elimina uno o varios archivos del espacio de trabajo e index.
* **git mv \<file(s)...> :** Mueve uno o varios archivos del espacio de trabajo e index.
* **git checkout -b \<name of new branch> :** Crea una rama y posiciona el HEAD allí.
* **git reset HEAD \<file(s)...> :** Descarta los archivos especificados del próximo commit. Restablece el index pero no el árbol de trabajo (ej:, los cambios en archivos se mantienen pero no se preparan para commit) y reporta cuales no han sido actualizados.
* **git diff --cached [\<commit>] :** Visualiza los cambios que se han preparado vs el último commit. Se puede pasar un \<commit> para ver los cambios relativos al mismo.
* **git commit [-m 'msg'] :** Almacena el contenido actual del index en un nuevo commit acompañado de un mensaje de log que describe esos cambios.
* **git commit --amend :** Modifica el último commit con los cambios actuales en el index.

![Git Directory][img8]

* **Repositorio local :** Un subdirectorio llamado .git que contiene todos los archivos necesarios — un esqueleto del repositorio Git. Ramas típicas: `main`, `feature-x`, `bugfix-y`
* **git diff \<commit or branch> :** Muestra los cambios que existen en el workspace relativos al commit mencionado. Puede usarse HEAD para comparar contra el último commit, o el nombre de una rama (branch) para comparar contra otra rama.
* **git commit [-m 'msg'] :** Almacena el contenido actual del index en un nuevo commit acompañado de un mensaje de log que describe esos cambios.
* **git reset --soft HEAD^ :** Deshace el último commit, dejando los cambio en el index.
* **git reset --hard :** Equipara el espacio de trabajo y el index al árbol local. ADVERTENCIA: Se pierden todos los cambios a archivos registrados por git desde el último commit. Usar este comando si una combinación/merge resultó en conflictos y es necesario comenzar de nuevo. Al pasar ORIG_HEAD puede deshacerse el merge más reciente y todos los cambios posteriores.
* **git reset --hard \<remote>/\<branch> :** Equipara el espacio de trabajo y el index con una rama remota. Usar reset ‑‑hard origin/main para descartar todos los commits en la rama local main. Se puede utilizar para comenzar de nuevo desde una combinación/merge fallida.
* **git switch \<branch> :** Cambia de rama actualizando el index y el espacio de trabajo para reflejar la rama especificada, branch, y actualizando la posición de HEAD a branch.
* **git checkout -b \<name of new branch> :** Crea una rama y posiciona el HEAD allí
* **git merge \<commit or branch> :** Combina (merge) los cambios de branch name con los de la rama actual. Usar ***‑‑no-commit*** para dejar los cambios sin realizar un commit.
* **git rebase upstream :** Revierte todos los commits desde que la rama actual se separó del \<upstream>, y luego los vuelve a aplicar uno por uno por sobre los commits del HEAD de \<upstream>.
* **git cherry-pick \<commit> :** Aplica los cambios del commit especificado en la rama actual.
* **git revert \<commit> :** Revierte el commit especificado y realiza un commit con el resultado. Esto requiere que el árbol de trabajo esté limpio (sin modificaciones desde el HEAD commit).
* **git diff --cached [\<commit>] :** Visualiza los cambios que se han preparado vs el último commit. Se puede pasar un \<commit> para ver los cambios relativos al mismo.
* **git commit [-m 'msg'] :** Almacena el contenido actual del index en un nuevo commit acompañado de un mensaje de log que describe esos cambios.
* **git commit --amend :** Modifica el último commit con los cambios actuales en el index.
* **git log :** Muestra los commits recientes, comenzando por los últimos. Optiones:
  * ***decorate*** para incluir nombres de ramas y tags
  * ***stat*** para incluir métricas (archivos modificados, insertados, and eliminados)
  * ***author=\<author>*** para filtrar por autor
  * ***after="MMM DD YYYY"*** ej. ("Jun 20 2008") para incluir commits desde esa fecha
  * ***before="MMM DD YYYY"*** incluye commits anteriores a esa fecha
  * ***merge*** incluye únicamente los commits involucrados en conflictos de combinación.
* **git diff \<commit> \<commit> :** Visualizar los cambios entre dos commits arbitrariamente.
* **git branch :** Lista todas las ramas existentes. Agregando -r lista las ramas registradas como remotas, la opción -a muestra ambas ramas.
* **git branch -d \<branch> :** Elimina la rama especificada. Usar -D para forzar esto.
* **git branch --track \<new> \<remote/branch> :** Crea una nueva rama local que sigue a una rama remota.
* **git fetch \<remote> \<refspec> :** Descarga los objetos y referencias desde otro repositorio.
* **git push :** Actualiza el servidor con los commits de todas ramas que tienen en *COMÚN* entre el repositorio local y el remoto. Las ramas locales que nunca fueron enviadas al server (push) no están compartidas.
* **git push \<remote> \<branch> :** Envía una nueva (o existente) rama al repositorio remoto
git rm.
* **git push \<remote> \<branch>:\<branch> :** Envía una rama al repositorio remoto con otro nombre.
* **git stash branch \<branchname> [\<stash>] :** Crea y posiciona HEAD en el \<branchname> apuntando al commit del cual el \<stash> fue creado originalmente, aplicando luego los cambios almacenados en el \<stash> al nuevo árbol de trabajo. 

  Si se realiza exitosamente, y stash es una referencia tipo *stash@{revision}*, el comando elimina el \<stash>. Cuando no se especifica un stash, aplica el último. 
  
  Este comando es útil en los casos en que la rama en la que se ejecutó git stash push ha cambiado demasiado por lo que git stash apply fallaría por conflictos. Al aplicar los cambios sobre el commit que fue HEAD al momento de ejecutar git stash originalmente, se restauran los cambios sin conflictos.

![Repositorio remoto][img9]

* **Repositorio remoto :** Version(es) del proyecto que están alojadas en Internet o una red, asegurando que todos los cambios están disponibles para otros desarrolladores. Por defecto es "origin". Ramas típicas aquí son: `main`, `shared-feature-x`, `release-y`
* **git branch -r :** Lista las ramas remotas
* **git branch --track \<new> \<remote/branch> :** Crea una nueva rama local que sigue a una rama remota.
* **git clone repo :** Descarga el repositorio especificado por repo y posiciona el HEAD en la rama main.
* **git pull \<remote> \<refspec> :** Incorpora los cambios desde un repositorio remoto en la rama actual. En su modo por defecto, ***git pull*** es un atajo de ***git fetch*** seguido por ***git merge***  FETCH_HEAD.
* **git fetch \<remote> \<refspec> :** Descarga los objetos y referencias desde otro repositorio.
* **git push :** Actualiza el servidor con los commits de todas ramas que tienen en *COMÚN* entre el repositorio local y el remoto. Las ramas locales que nunca fueron enviadas al server (push) no están compartidas.
* **git push \<remote> \<branch> :** Envía una nueva (o existente) rama al repositorio remoto
git rm.
* **git push \<remote> \<branch>:\<branch> :** Envía una rama al repositorio remoto con otro nombre.
* **git push \<remote> --delete \<branch> :** Elimina una rama remota.

[img1]: ./img/snapshots.png "img 1. Almacenamiento de datos como instantáneas del proyecto a través del tiempo"
[img2]: ./img/areas.png "img 2. Flujo de trabajo en Git"
[img3]: ./img/lifecycle.png "img 3. Ciclo de vida del estado de los archivos"
[img4]: ./img/estados-git.webp "img 4. Estados GIT"
[img5]: ./img/stash.PNG "img 5. Stash"
[img6]: ./img/workspace.PNG "img 6. Workspace"
[img7]: ./img/index.PNG "img 7. Index"
[img8]: ./img/gitdirectory.PNG "img 8. Directorio local"
[img9]: ./img/repositorioRemoto.PNG "img 9. Repositorio remoto"

## 5.  Ramificaciones en Git

Cuando hablamos de ramificaciones, significa que tú has tomado la rama principal de desarrollo (main) y a partir de ahí has continuado trabajando sin seguir la rama principal de desarrollo.

* **git reset \<commit> :** Permite retornar a una versión anterior.

![Git rm y Git reset][img10]

El comando git reset es una herramienta poderosa que te permite deshacer o revertir cambios en tu repositorio de Git. Lo puedes ejecutar de tres maneras diferentes, con las líneas de comando --soft, --mixed y --hard.

Pero no como git checkout que nos deja ir, mirar, pasear y volver. Con git reset volvemos al pasado sin la posibilidad de volver al futuro. Borramos la historia y la debemos sobreescribir. No hay vuelta atrás.

* git reset --soft: Borra el historial y los registros de Git de commits anteriores, pero guarda los cambios en Staging para aplicar las últimas actualizaciones a un nuevo commit.
* git reset --hard: Deshace todo, absolutamente todo. Toda la información de los commits y del área de staging se elimina del historial.
* git reset --mixed: Borra todo, exactamente todo. Toda la información de los commits y del área de staging se elimina del historial.
* git reset HEAD: El comando git reset saca archivos del área de staging sin borrarlos ni realizar otras acciones. Esto impide que los últimos cambios en estos archivos se envíen al último commit. Podemos incluirlos de nuevo en staging con git add si cambiamos de opinión.

  Ten en cuenta que, si deshaces commits en un repositorio compartido en GitHub, estarás cambiando su historia y esto puede causar problemas de sincronización con otros colaboradores.

Por otro lado, git rm es un comando que nos ayuda a eliminar archivos de Git sin eliminar su historial del sistema de versiones. Para recuperar el archivo eliminado, necesitamos retroceder en la historia del proyecto, recuperar el último commit y obtener la última confirmación antes de la eliminación del archivo.

Es importante tener en cuenta que git rm no puede usarse sin evaluarlo antes. Debemos usar uno de los flags siguientes para indicarle a Git cómo eliminar los archivos que ya no necesitamos en la última versión del proyecto.

## **Variaciones de Git rm**

* git rm --cached: Elimina archivos del repositorio local y del área de staging, pero los mantiene en el disco duro. Deja de trackear el historial de cambios de estos archivos, por lo que quedan en estado untracked.
* git rm --force: Elimina los archivos de Git y del disco duro. Git guarda todo, por lo que podemos recuperar archivos eliminados si es necesario (empleando comandos avanzados).

¡Al usar git rm lo que haremos será eliminar este archivo completamente de git!

* **git log --stat :** ver los cambios específicos en cuales archivos a partir del commit.
* **git checkout \<commit> \<file> :** devolvernos al commit mencionado. Si se hace el commit se borra lo realizado después del commit restaurado. Si traigo el commit master me restaura a la versión actual.

[img10]: ./img/arboles-git.webp "img 10. rm y reset"

Algunos comandos que pueden ayudar cuando colaboren con proyectos muy grandes de github:

* git log --oneline - Te muestra el id commit y el título del commit.
* git log --decorate- Te muestra donde se encuentra el head point en el log.
* git log --stat - Explica el número de líneas que se cambiaron brevemente.
* git log -p- Explica el número de líneas que se cambiaron y te muestra que se cambió en el contenido.
* git shortlog - Indica que commits ha realizado un usuario, mostrando el usuario y el titulo de sus commits.
* git log --graph --oneline --decorate y
git log --pretty=format:"%cn hizo un commit %h el dia %cd" - Muestra mensajes personalizados de los commits.
* git log -3 - Limitamos el número de commits.
* git log --after=“2018-1-2” ,
* git log --after=“today” y
* git log --after=“2018-1-2” --before=“today” - Commits para localizar por fechas.
* git log --author=“Name Author” - Commits realizados por autor que cumplan exactamente con el nombre.
* git log --grep=“INVIE” - Busca los commits que cumplan tal cual está escrito entre las comillas.
* git log --grep=“INVIE” –i- Busca los commits que cumplan sin importar mayúsculas o minúsculas.
* git log – index.html- Busca los commits en un archivo en específico.
* git log -S “Por contenido”- Buscar los commits con el contenido dentro del archivo.
* git log > log.txt - guardar los logs en un archivo txt

## RAMAS
[Flujo de trabajo Altasian](https://www.atlassian.com/es/git/tutorials/comparing-workflows/gitflow-workflow)

git remote add origin [url]

git remote

$ git remote -v
origin  git@github.com:mpovedam/ApuntesGit.git (fetch)  # enviar cosas
origin  git@github.com:mpovedam/ApuntesGit.git (push)   # Recibir cosas

git branch -m main -> para cambiar la rama master por main

```GIT
$ git push origin main
The authenticity of host 'github.com (140.82.114.4)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
Enter passphrase for key '/c/Users/Azulito/.ssh/id_rsa':
To github.com:mpovedam/ApuntesGit.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:mpovedam/ApuntesGit.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally. This is usually caused by another repository pushing to
hint: the same ref. If you want to integrate the remote changes, use
hint: 'git pull' before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

Debo traer primero los archivos del repositorio con un pull

git pull origin main

fatal: refusing to merge unrelated histories  -> para forzar las historias se hace lo siguiente

```Git
$ git pull origin main --allow-unrelated-histories
Enter passphrase for key '/c/Users/Azulito/.ssh/id_rsa':
From github.com:mpovedam/ApuntesGit
 * branch            main       -> FETCH_HEAD
error: Your local changes to the following files would be overwritten by merge:
  img/Git rm Git Reset.webp img/arboles-git.webp
Merge with strategy ort failed.
```
