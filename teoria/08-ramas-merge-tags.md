# Ramas, merge y tags

## Ramas
~~~bash
git branch
git switch -c nueva-funcion
git switch main
git branch -a
git push -u origin nueva-funcion
~~~
- git branch lista las ramas locales; el asterisco marca la actual.
- git switch -c crea la rama y se cambia a ella (equivale a git checkout -b). La rama nueva sale del commit en el que estás.
- git switch cambia de rama. Haz commit antes, para no arrastrar cambios a medias.
- git branch -a muestra también las ramas remotas (remotes/origin/...).
- La primera vez que subes una rama, push -u origin nombre; después basta con git push.

## Merge: integrar una rama en otra
Te pones en la rama que **recibe** los cambios y nombras la que los **trae**:
~~~bash
git switch main
git merge nueva-funcion
git log --oneline --graph --all
git push
~~~
- **Fast-forward**: si main no ha avanzado desde que creaste la rama, Git solo mueve main hacia delante y no crea ningún commit nuevo.
- **Commit de merge**: si las dos ramas tienen commits propios, Git crea un commit que une las dos historias. Si abre el editor para el mensaje, en nano guarda con Ctrl+O, Enter y sal con Ctrl+X.
- git merge --no-ff nueva-funcion crea el commit de merge aunque se pudiera hacer fast-forward; así el historial muestra que hubo una rama.
- git merge -m "Mensaje" nueva-funcion pone el mensaje del commit de merge sin abrir el editor.
- --graph dibuja las ramas en el log.

## Conflictos
Si las dos ramas cambian las mismas líneas de un fichero, el merge se para y Git marca el fichero así:
~~~text
<<<<<<< HEAD
Texto que había en main
=======
Texto que trae nueva-funcion
>>>>>>> nueva-funcion
~~~
1. git status muestra los ficheros en conflicto (both modified).
2. Edita el fichero: deja el texto final y borra las tres líneas de marcas (<<<<<<<, ======= y >>>>>>>).
3. git add fichero
4. git commit (sin -m usa el mensaje que propone Git).

Para cancelar el merge y dejarlo todo como estaba: git merge --abort.

## Borrar ramas
~~~bash
git branch -d nueva-funcion
git push origin --delete nueva-funcion
~~~
- -d solo borra la rama si ya está integrada; -D la borra igualmente y pierdes los commits que no estén en otra rama.
- push origin --delete borra la rama en GitHub.

## Merge desde GitHub (pull request)
Al pulsar **Merge pull request**, el merge se hace en GitHub, no en tu ordenador. Para tenerlo también en local:
~~~bash
git switch main
git pull
git branch -d nueva-funcion
~~~
Si el pull request se integró con **Squash and merge**, -d dice que la rama no está integrada: compruébalo en GitHub y bórrala con -D.

## Tags: marcar versiones
Un tag es un nombre fijo para un commit concreto, normalmente una versión (v1.0). A diferencia de una rama, no avanza cuando haces commits nuevos.
~~~bash
git tag v1.0
git tag -a v1.1 -m "Versión 1.1"
git tag -a v0.9 -m "Versión 0.9" a1b2c3d
git tag
git show v1.1
~~~
- **Ligero** (git tag v1.0): solo un nombre que apunta al commit.
- **Anotado** (-a y -m): guarda además autor, fecha y mensaje. Es el recomendado para versiones.
- Sin hash, el tag va al commit actual; con un hash, a ese commit (lo sacas de git log --oneline).
- git tag lista los tags; git tag -l "v1.*" filtra; git show enseña el tag y su commit.
- Para ver el código de una versión: git checkout v1.0 (o git switch --detach v1.0). Quedas fuera de cualquier rama (*detached HEAD*); vuelve con git switch main.

## Subir y borrar tags
git push **no** sube los tags; hay que subirlos aparte:
~~~bash
git push origin v1.1
git push origin --tags
git tag -d v1.0
git push origin --delete v1.0
~~~
- push origin v1.1 sube uno; --tags sube todos.
- tag -d borra solo el local; para borrarlo en GitHub, push origin --delete.
- En GitHub se ven en la página del repositorio → **Tags**. Una **Release** es un tag con notas y archivos descargables: **Releases → Draft a new release** y eliges el tag.

## Tags en GitHub Actions
Workflow que se ejecuta solo al subir un tag de versión:
~~~yaml
name: Publicar versión
on:
  push:
    tags:
      - 'v*'
jobs:
  version:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "Versión ${{ github.ref_name }}"
~~~
- 'v*' recoge v1.0, v2.3.1... Ponlo entre comillas: un patrón que empieza por * sin comillas rompe el YAML.
- Si solo pones tags, un push normal a main no lo dispara. Para que salte con las dos cosas, añade branches: [main] junto a tags.
- github.ref_name contiene el nombre del tag (o de la rama) que ha disparado el workflow.

## Errores típicos
| Mensaje | Causa | Solución |
|---|---|---|
| Already up to date | La rama ya estaba integrada, o estás en la rama equivocada | Mira en qué rama estás con git branch |
| CONFLICT (content) ... Automatic merge failed | Las dos ramas tocan las mismas líneas | Edita el fichero, git add y git commit |
| the branch '...' is not fully merged | -d con una rama sin integrar | Haz antes el merge, o -D si de verdad quieres perderla |
| tag 'v1.0' already exists | Ese nombre ya existe | Usa otro, o git tag -d v1.0 y créalo otra vez |
| El tag no aparece en GitHub | git push no sube tags | git push origin v1.0 |
| a branch is expected, got tag | git switch solo cambia a ramas | git switch --detach v1.0 o git checkout v1.0 |

## Comprueba que sabes
- [ ] Crear una rama, hacer un commit en ella y volver a main.
- [ ] Hacer un merge fast-forward y otro con commit de merge (--no-ff).
- [ ] Provocar un conflicto y resolverlo a mano.
- [ ] Crear un tag anotado en el commit actual y otro en un commit antiguo.
- [ ] Subir un tag a GitHub y borrarlo.
- [ ] Escribir un workflow que se active al subir un tag v*.
