# Práctica 5. GitHub Flow: issues, ramas, pull requests y Pages

## Objetivo
Seguir un flujo de trabajo colaborativo: describir una tarea, desarrollarla en una rama independiente, pedir revisión e integrar el cambio. Después, publicar la web desde `docs/`.

**Teoría:** [Ramas, merge y tags](../teoria/04-ramas-merge-tags.md) · [Pages y despliegues](../teoria/08-pages-despliegues.md)

## 1. Clonar el repositorio
```bash
git clone https://github.com/antoni-gimenez/gitFlow.git
cd gitFlow
git status
```
Si no tienes permisos de escritura en el repositorio original, trabaja desde un fork o una copia autorizada. No intentes publicar cambios directamente donde no tienes permisos.

## 2. Crear una issue
En GitHub entra en **Issues → New issue**. Escribe un título claro, explica qué hay que cambiar y crea la issue. Anota su número, porque después se enlazará al pull request.

## 3. Crear una rama para el cambio
```bash
git switch -c feature_nombre_alumno
```
Usa el nombre exacto indicado por el profesor. La rama permite trabajar sin modificar directamente la rama principal.

## 4. Editar, revisar y confirmar
Haz el cambio solicitado y pruébalo. Antes de confirmar:
```bash
git status
git diff
git add .
git status
git commit -m "Añadir funcionalidad solicitada"
git push -u origin feature_nombre_alumno
```
`git diff` ayuda a revisar las modificaciones; `git add` prepara los archivos; `commit` crea una versión y `push` publica la rama.

Por HTTPS, GitHub no acepta la contraseña de la cuenta: usa un token personal como contraseña o `gh auth login`. Si el push falla, consulta los [errores típicos](../teoria/03-repos-de-cero.md#errores-típicos).

## 5. Abrir un Pull Request
En GitHub abre **Pull requests → New pull request**. Selecciona la rama de la funcionalidad como origen (*compare*) y `main` como destino (*base*), salvo que el enunciado diga otra cosa. Describe el cambio y vincula la issue escribiendo, por ejemplo, `Closes #12` con el número real. Solicita la revisión requerida y corrige las observaciones antes del merge.

El merge del pull request se hace en GitHub. Después, actualiza tu copia local y borra la rama:
```bash
git switch main
git pull
git branch -d feature_nombre_alumno
git log --oneline --graph
```
Si el enunciado pide integrar la rama desde la terminal en vez de con un pull request, o marcar la entrega con un tag, mira [Ramas, merge y tags](../teoria/04-ramas-merge-tags.md).

## 6. Publicar desde la carpeta docs
Verifica que `docs/index.html` existe y que sus recursos se cargan. En **Settings → Pages**, elige **Deploy from a branch**, selecciona la rama indicada (normalmente `main`) y la carpeta `/docs`. Guarda y espera a que termine el despliegue.

## 7. Comprobaciones para entregar
Comprueba que el pull request está integrado, que la issue se ha cerrado cuando corresponda y que la web publicada muestra el contenido correcto. Guarda las capturas pedidas. No hagas push directo a ramas protegidas ni cierres una issue sin comprobar el resultado.

---
[← Práctica 4](04-Compilar-C-con-Makefile-y-guardar-artefacto.md) · [Índice de prácticas](README.md)
