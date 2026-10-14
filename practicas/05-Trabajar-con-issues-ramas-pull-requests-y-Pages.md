# Práctica 5. Trabajar con issues, ramas, pull requests y GitHub Pages

## Objetivo
Practicar el flujo de trabajo de GitHub: issue, rama, cambios, commit, push, pull request, revisión e integración. Después, comprobar la publicación de la web desde la carpeta `docs/`.

> Usa la URL, nombres de ramas y estructura exactos del enunciado del profesor. Los valores de este documento son una guía si el enunciado no concreta otro nombre.

## 1. Clonar el repositorio

```bash
git clone https://github.com/antoni-gimenez/gitFlow.git
cd gitFlow
git status
git remote -v
git branch --show-current
```

- `git clone`: descarga el repositorio y su historial.
- `cd gitFlow`: entra en la carpeta creada.
- `git status`: muestra el estado de la copia local.
- `git remote -v`: enseña las URL de descarga y subida.
- `git branch --show-current`: indica la rama actual.

Si el enunciado proporciona otra URL, reemplaza la de ejemplo. Si no tienes permiso para subir cambios al repositorio original, sigue las instrucciones del profesor para crear un fork o usar una copia autorizada.

## 2. Crear una issue en GitHub

1. Abre el repositorio en GitHub.
2. Entra en **Issues → New issue**.
3. Escribe un título que describa el cambio.
4. En el cuerpo, explica qué se debe modificar y cómo se comprobará.
5. Pulsa **Submit new issue**.
6. Anota el número que GitHub asigna, por ejemplo `#3`. No inventes el número: usa el que aparezca en tu repositorio.

Una issue sirve para registrar y seguir una tarea o un error.

## 3. Crear la rama de trabajo

Comprueba primero la rama actual y los cambios:

```bash
git status
git branch --show-current
git switch -c feature_nombre_alumno
git branch --show-current
```

- `git switch -c nombre`: crea una rama nueva y cambia a ella.
- `git branch --show-current`: confirma que estás trabajando en la rama correcta.
- Sustituye `feature_nombre_alumno` por el nombre exacto pedido.

Si la rama ya existe, usa `git switch feature_nombre_alumno` en lugar de volver a crearla.

## 4. Revisar la estructura de la web

```bash
ls -la
find docs -maxdepth 3 -type f
test -f docs/index.html && echo "Página principal encontrada"
```

- `ls -la`: muestra los archivos de la raíz.
- `find docs ...`: lista los archivos que hay en `docs/`.
- `test -f docs/index.html`: comprueba que exista la página principal de la publicación.

Si el repositorio ya contiene la web del enunciado, edita esos archivos; no los sustituyas por un ejemplo.

## 5. Crear una página mínima solo si hace falta

Si el enunciado pide publicar una página y todavía no existe, puedes crear este ejemplo:

```bash
mkdir -p docs
cat > docs/index.html <<'EOF'
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Práctica GitHub Flow</title>
</head>
<body>
  <h1>Práctica GitHub Flow</h1>
  <p>Página publicada desde docs.</p>
</body>
</html>
EOF
cat docs/index.html
```

- `mkdir -p docs`: crea la carpeta si no existe.
- `cat > docs/index.html`: crea el archivo HTML.
- `cat docs/index.html`: muestra el contenido guardado.

## 6. Editar los archivos que pide la issue

Modifica los archivos solicitados por la tarea. Después revisa el estado y las diferencias:

```bash
git status
git diff
```

- `git status`: lista los archivos modificados, nuevos o eliminados.
- `git diff`: muestra los cambios que todavía no están preparados.
- Abre la web en el navegador y comprueba el cambio antes de confirmar.

## 7. Preparar y crear el commit

```bash
git add .
git status
git diff --staged
git commit -m "Añadir cambio solicitado"
git log --oneline -3
```

- `git add .`: prepara los cambios de la carpeta actual y sus subcarpetas. No lo uses sin revisar si hay archivos que no deban subirse.
- `git status`: confirma qué archivos están preparados.
- `git diff --staged`: permite revisar el contenido que entrará en el commit.
- `git commit -m "..."`: guarda los cambios en el historial.
- `git log --oneline -3`: muestra los últimos tres commits.

## 8. Subir la rama

```bash
git push -u origin feature_nombre_alumno
```

- `git push`: sube los commits al remoto.
- `-u origin feature_nombre_alumno`: configura el seguimiento para que futuros `git push` y `git pull` puedan usar la rama predeterminada.
- Sustituye el nombre por el que utilizaste al crear la rama.

## 9. Abrir el Pull Request

1. En GitHub abre **Pull requests → New pull request**.
2. En la rama de origen (*compare*), elige `feature_nombre_alumno`.
3. En la rama de destino (*base*), elige la rama que indique el enunciado, normalmente `main).
4. Añade título y descripción.
5. Enlaza la issue escribiendo `Closes #NUMERO`, sustituyendo `NUMERO` por el número real de la issue.
6. Pulsa **Create pull request**.
7. Revisa los cambios en la pestaña **Files changed**.
8. Corrige cualquier observación y espera la revisión o aprobación requerida.

Un pull request propone integrar una rama en otra; crearlo no significa que ya esté integrado.

## 10. Integrar y comprobar

Cuando se cumplan los requisitos del ejercicio, integra el pull request con el método permitido por el repositorio. Después comprueba en GitHub que aparece como **Merged** y que la issue se ha cerrado si estaba vinculada con `Closes #NUMERO`.

Si necesitas actualizar tu copia local después de integrar:

```bash
git switch main
git pull
git status
```

- `git switch main`: cambia a la rama principal local.
- `git pull`: descarga e integra los cambios de la rama remota seguida por `main).
- `git status`: comprueba el estado final.

## 11. Publicar desde docs con GitHub Pages

1. Abre **Settings → Pages**.
2. En **Build and deployment**, selecciona **Deploy from a branch**.
3. Selecciona la rama integrada que se debe publicar, normalmente `main`.
4. Selecciona la carpeta `/docs`.
5. Pulsa **Save** y espera al despliegue.
6. Abre la URL que muestra GitHub Pages y comprueba que la web carga.

La carpeta `docs/` debe estar en la raíz de la rama que se publica y debe contener `index.html).

## 12. Lista de comprobación para entregar

- [ ] La issue existe y describe el trabajo.
- [ ] El cambio se hizo en una rama de funcionalidad.
- [ ] Los commits tienen mensajes claros.
- [ ] La rama se subió a GitHub.
- [ ] El pull request apunta a las ramas correctas.
- [ ] El pull request se revisó e integró si el enunciado lo exige.
- [ ] La issue se cerró cuando correspondía.
- [ ] Pages publica desde la rama y carpeta correctas.
- [ ] La URL pública carga la web.
- [ ] Se han tomado las capturas exigidas por el profesor.
