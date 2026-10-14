# Práctica 5. Sistema de control de versiones, ramas, imagen y publicación

## Objetivo
Trabajar en equipo con Git y GitHub usando ramas para desarrollar cambios HTML, atributos de una imagen y estilos CSS; integrar versiones, preparar una release, aplicar un hotfix y publicar la documentación/web.

> Usa la URL real que te haya dado el profesor. No inventes una URL ni subas directamente a una rama protegida. Los nombres de ramas de este guion se conservan tal como aparecen en el material de la práctica.

## 1. Clonar el repositorio

Copia la URL HTTPS del repositorio del ejercicio desde GitHub y sustituye `URL_DEL_REPOSITORIO`:

```bash
git clone URL_DEL_REPOSITORIO
cd practica-sistema-control-versiones
git status
git remote -v
git branch -a
```

- `git clone`: descarga el repositorio y el historial.
- `cd practica-sistema-control-versiones`: entra en la carpeta local. Si la carpeta se ha creado con otro nombre, usa ese nombre.
- `git status`: comprueba que no haya cambios locales.
- `git remote -v`: comprueba el remoto.
- `git branch -a`: muestra las ramas locales y remotas.

## 2. Comprobar los archivos del proyecto

```bash
find . -maxdepth 4 -type f
```

La estructura esperada incluye:

```text
index.html
css/styles.css
js/main.js
assets/images/
README.md
```

- `index.html`: contenido HTML de la web.
- `css/styles.css`: reglas de presentación.
- `js/main.js`: comportamiento JavaScript.
- `assets/images/`: carpeta de imágenes.
- `README.md`: documentación del repositorio.

No borres ni sustituyas archivos que ya haya preparado el profesor.

## 3. Crear y publicar la rama develop

Primero actualiza la rama principal y crea `develop`:

```bash
git switch main
git pull origin main
git switch -c develop
git push -u origin develop
git branch --show-current
```

- `git switch main`: cambia a la rama principal.
- `git pull origin main`: actualiza la rama local con los cambios remotos.
- `git switch -c develop`: crea la rama de integración y cambia a ella.
- `git push -u origin develop`: publica la rama y configura el seguimiento.
- `git branch --show-current`: comprueba la rama actual.

Si `develop` ya existe, utiliza `git switch develop` y `git pull origin develop`; no intentes crearla otra vez.

## 4. Cambiar el contenido HTML en una rama de funcionalidad

Crea la rama desde `develop`:

```bash
git switch develop
git pull origin develop
git switch -c feature/contenidoHTML
```

Edita `index.html` para añadir o mejorar el contenido que indique el enunciado. Comprueba el archivo:

```bash
cat index.html
git diff
```

- `cat index.html`: muestra el HTML.
- `git diff`: enseña los cambios que todavía no están preparados.

Guarda y sube el cambio:

```bash
git add index.html
git diff --staged
git commit -m "feat: mejorar contenido HTML"
git push -u origin feature/contenidoHTML
```

- `git add index.html`: prepara el HTML.
- `git diff --staged`: permite revisar los cambios preparados.
- `git commit`: crea un commit.
- `git push -u`: publica la rama de funcionalidad.

Después, en GitHub crea un Pull Request desde `feature/contenidoHTML` hacia `develop`. Revisa los cambios y, cuando proceda, integra el PR. Actualiza tu copia local:

```bash
git switch develop
git pull origin develop
```

## 5. Añadir o corregir la imagen desde una rama de funcionalidad

Crea la rama requerida:

```bash
git switch develop
git pull origin develop
git switch -c feature/atributosHTML
```

Comprueba que existe la carpeta de imágenes y localiza los archivos:

```bash
mkdir -p assets/images
find assets/images -maxdepth 2 -type f
```

Copia la imagen facilitada por el profesor dentro de `assets/images/`. Por ejemplo, si está en Descargas y se llama `foto.jpg`:

```bash
cp ~/Descargas/foto.jpg assets/images/foto.jpg
ls -lh assets/images/
```

Cambia la ruta de origen por la ubicación real. Si el archivo tiene otro nombre o extensión, usa ese nombre en el HTML.

En `index.html`, añade o corrige el atributo `src` de la etiqueta de imagen. Por ejemplo:

```html
<img src="assets/images/foto.jpg" alt="Descripción de la fotografía">
```

- `src`: ruta relativa de la imagen dentro del repositorio.
- `alt`: descripción textual de la imagen.
- La ruta, el nombre y las mayúsculas deben coincidir exactamente con el archivo que has copiado.

Revisa los cambios:

```bash
git status
git diff
```

Guarda el HTML y la imagen en el mismo commit:

```bash
git add index.html assets/images/
git diff --staged
git commit -m "feat: añadir imagen a la web"
git push -u origin feature/atributosHTML
```

Después crea un Pull Request de `feature/atributosHTML` hacia `develop`, revisa los archivos modificados e intégralo cuando corresponda. Actualiza `develop`:

```bash
git switch develop
git pull origin develop
```

## 6. Modificar los estilos CSS desde otra rama

```bash
git switch develop
git pull origin develop
git switch -c feature/estilosCSS
```

Edita `css/styles.css` con los estilos que pida el ejercicio. Comprueba la ruta y el contenido:

```bash
ls -l css/
cat css/styles.css
git diff
```

Guarda el cambio:

```bash
git add css/styles.css
git diff --staged
git commit -m "feat: mejorar estilos CSS"
git push -u origin feature/estilosCSS
```

Crea el Pull Request desde `feature/estilosCSS` hacia `develop`; revisa e integra el cambio. Después:

```bash
git switch develop
git pull origin develop
```

Abre `index.html` en el navegador y comprueba que el HTML, la imagen y los estilos se ven correctamente. Si el HTML no enlaza la hoja de estilos, comprueba que contiene una referencia similar a:

```html
<link rel="stylesheet" href="css/styles.css">
```

## 7. Preparar la release 1.0

Cuando las funcionalidades de `develop` estén terminadas y revisadas, crea la rama de release:

```bash
git switch develop
git pull origin develop
git switch -c release/1.0
git push -u origin release/1.0
```

- `release/1.0`: rama para revisar y preparar la versión 1.0.
- En esta rama solo se deberían hacer los ajustes finales que permita el enunciado.

Crea un Pull Request desde `release/1.0` hacia `main`. Cuando se apruebe e integre, etiqueta la versión en `main`:

```bash
git switch main
git pull origin main
git tag -a v1.0 -m "Version 1.0"
git push origin v1.0
git tag
```

- `git tag -a v1.0`: crea una etiqueta anotada para identificar la versión.
- `git push origin v1.0`: publica la etiqueta.
- `git tag`: lista las etiquetas locales.

Si el profesor pide integrar también la release en `develop`, crea el PR correspondiente o sigue el flujo indicado en el enunciado.

## 8. Crear un hotfix a partir de main

Para corregir un problema urgente en la versión publicada:

```bash
git switch main
git pull origin main
git switch -c hotfix/millorasV_1_0
```

Realiza la corrección indicada por el profesor en `index.html). Revisa y confirma:

```bash
git diff
git add index.html
git diff --staged
git commit -m "fix(usuario-1): mejorar ejemplo de contenido HTML"
git push -u origin hotfix/millorasV_1_0
```

Crea un Pull Request desde `hotfix/millorasV_1_0` hacia `main`. Cuando se integre, actualiza la etiqueta o crea la etiqueta de corrección solo si lo exige el enunciado. Integra también el arreglo en `develop` para evitar que la corrección se pierda en futuras versiones.

## 9. Publicar la web/documentación en gh-pages

Comprueba si el repositorio ya tiene la rama de publicación:

```bash
git branch -a
```

Si el enunciado exige publicar mediante la rama `gh-pages`, créala a partir de la rama y el contenido que el profesor indique. No publiques desde una rama distinta sin comprobar los requisitos.

Si hay que crearla desde `main`:

```bash
git switch main
git pull origin main
git switch --orphan gh-pages
```

Una rama huérfana no tiene el historial anterior. **No ejecutes los comandos de limpieza a ciegas**: revisa primero qué archivos deben publicarse y sigue la estructura exigida por el profesor. Añade únicamente los archivos destinados al sitio y publica:

```bash
git status
git add index.html css/ js/ assets/ README.md
git diff --staged
git commit -m "docs: publicar sitio web"
git push -u origin gh-pages
```

Si el profesor pide usar `docs/` desde `main` en lugar de `gh-pages`, configura **Settings → Pages → Deploy from a branch**, selecciona `main` y `/docs`. No mezcles ambos métodos: usa el que diga el enunciado.

En GitHub, abre **Settings → Pages** y comprueba el método de publicación configurado. Abre la URL que indique GitHub y verifica que la web y la imagen se cargan.

## 10. Comprobación final

```bash
git status
git branch -a
git log --oneline --decorate --graph --all
git tag
```

- `git status`: comprueba si quedan cambios pendientes.
- `git branch -a`: muestra las ramas locales y remotas.
- `git log --oneline --decorate --graph --all`: muestra visualmente el historial y las ramas.
- `git tag`: comprueba las etiquetas creadas.

Antes de entregar, comprueba que los Pull Requests se han integrado, que existe la etiqueta `v1.0` si se exige, que el hotfix está integrado donde corresponde y que la web publicada muestra la foto correctamente.
