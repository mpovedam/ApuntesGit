# Caso de uso

Como punto de partida trabajaremos en la creación de esta guía donde se creó el archivo *note.md*, se ejecuto *git init* y luego *git add note.md*. Que sería nuestra primer instantánea del proyecto.

**Revisando el Estado de los archivos:**

``` Git
$ git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   note.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   note.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        ejemplo_1/
        img/
```

Se puede ver que **ejemplo_1/\*** y **img/\*** están *untracked*.

* **Rastrear Archivos Nuevos:** También se ve que **note.md** etá siendo rastreado *Changes to be committed (Cambios a ser confirmados)*.

* **Preparar Archivos Modificados:** Pero también se ve que **note.md** aparece en la sección llamada *Changes not staged for commit(Cambios no preparado para confirmar)* - lo que significa que existe un archivo rastreado que ha sido modificado en el directorio de trabajo pero que aún no está preparado.

    Para prepararlo, se ejecuta *git add .* donde el punto indica todos los archivos desde la ruta donde me encuentro o también podría llamar solo el archivo *git add note.md*

> ***git add*** es un comando que cumple varios propósitos - lo usas para empezar a rastrear archivos nuevos, preparar archivos, y hacer otras cosas como marcar archivos en conflicto por combinación como resueltos.

Cómo eran varios archivos ejecuté:

``` Git
$ git add .

$ git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   .vscode/extensions.json
        new file:   .vscode/settings.json
        new file:   ejemplo_1/ejemplo_1.md
        new file:   img/areas.png
        new file:   img/lifecycle.png
        new file:   img/snapshots.png
        new file:   note.md
```

si se ejecuta la versión abreviada *git status -s* o *git status --short*

``` Git
$ git status --short
A  .vscode/extensions.json
A  .vscode/settings.json
AM ejemplo_1/ejemplo_1.md
A  img/areas.png
A  img/lifecycle.png
A  img/snapshots.png
AM note.md
?? ejemplo_1/archivoNuevo.txt
```

Donde los símbolos representan:

* **??**  *untraked files*
* **A**   *staged files*
* **M**   *modified files*

Las dobles columnas indican, la de la izquierda el estado del **staging area** (área preparada) y la de la derecha el estado del **working tree** (área sin preparar), por lo que *note.md* y *ejemplo_1/ejemplo_1.md* están preparados y modificados otra vez por lo que existen cambios sin preparar.

``` Git
$ git commit -m "Primer commit de la guía básica sobre GIT"
[main (root-commit) c84cb6e] Primer commit de la guía básica sobre GIT
 8 files changed, 212 insertions(+)
 create mode 100644 .vscode/extensions.json
 create mode 100644 .vscode/settings.json
 create mode 100644 ejemplo_1/archivoNuevo.txt
 create mode 100644 ejemplo_1/ejemplo_1.md
 create mode 100644 img/areas.png
 create mode 100644 img/lifecycle.png
 create mode 100644 img/snapshots.png
 create mode 100644 note.md
```

``` Git
$ git log
commit c84cb6eb8e6ee1347676d4cce08b91e7fa962d9f (HEAD -> main)
Author: Mauricio Poveda <mauriciomacias2.0@gmail.com>
Date:   Mon Aug 21 00:01:55 2023 -0500

    Primer commit de la guía básica sobre GIT
```
