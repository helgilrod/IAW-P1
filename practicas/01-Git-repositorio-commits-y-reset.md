# Práctica 1. Crear un repositorio Git, guardar commits y utilizar reset

## Objetivo
Crear un repositorio local, preparar archivos, guardar versiones, consultar el historial y distinguir `reset --soft`, `reset --mixed` y `reset --hard`.

> Haz los ejercicios de `reset` en este repositorio de prueba. No ejecutes `reset --hard` en un proyecto con trabajo que quieras conservar.

## 1. Crear el proyecto

Desde la terminal:

```bash
mkdir activitatsPrimeraClaseDAW
cd activitatsPrimeraClaseDAW
pwd
git init
git branch -M main
git status
```

- `mkdir activitatsPrimeraClaseDAW`: crea la carpeta.
- `cd activitatsPrimeraClaseDAW`: entra en ella.
- `pwd`: muestra la ruta actual.
- `git init`: crea el repositorio local en la carpeta oculta `.git`.
- `git branch -M main`: establece `main` como nombre de la rama actual.
- `git status`: muestra la rama y el estado de los archivos.

## 2. Configurar nombre, correo y editor

Hazlo solo si todavía no está configurado:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
git config --global core.editor nano
git config --global --list
```

- `user.name`: nombre que aparecerá como autor de los commits.
- `user.email`: correo asociado a los commits.
- `core.editor nano`: establece Nano como editor de Git.
- `--global --list`: muestra la configuración global. Sustituye los ejemplos por los datos que te indiquen.

## 3. Crear un archivo y comprobar el estado

```bash
echo "# Activitats primera clase DAW" > README.md
cat README.md
git status
```

- `echo "..." > README.md`: crea el archivo y escribe el título; `>` sobrescribe el contenido si ya existe.
- `cat README.md`: muestra su contenido.
- `git status`: Git detectará el archivo como *untracked* porque aún no se ha añadido.

## 4. Añadir y retirar un archivo del staging

```bash
git add README.md
git status
git restore --staged README.md
git status
```

- Primer `git add README.md`: prepara el archivo para el próximo commit.
- Primer `git status`: debe mostrar el archivo preparado para confirmar.
- `git restore --staged README.md`: lo saca del staging, pero no borra el archivo.
- Segundo `git status`: muestra el archivo otra vez como cambio no preparado.

## 5. Crear el primer commit

```bash
git add README.md
git diff --staged
git commit -m "Afegit fitxer Readme"
git log --oneline
```

- `git add README.md`: prepara el archivo.
- `git diff --staged`: muestra los cambios preparados que entrarán en el commit.
- `git commit -m "..."`: guarda una versión en el historial.
- `git log --oneline`: muestra los commits resumidos en una línea.

Para mostrar hash, autor y fecha:

```bash
git log --pretty=format:"%h %an %ad" --date=iso
```

- `%h`: hash abreviado.
- `%an`: nombre del autor.
- `%ad`: fecha del commit.
- `--date=iso`: presenta la fecha en formato ISO.

## 6. Crear cambios para probar reset

```bash
echo "Primera modificación" >> README.md
git add README.md
git commit -m "Primera modificacion"
echo "Segunda modificación" >> README.md
git add README.md
git commit -m "Segunda modificacion"
git log --oneline -4
```

- `>>` añade texto al final sin borrar el contenido anterior.
- Cada pareja `git add` + `git commit` guarda una versión independiente.
- `git log --oneline -4` muestra los cuatro últimos commits.

### 6.1 Probar `reset --soft`

```bash
git reset --soft HEAD~1
git status
git log --oneline -4
```

- `HEAD~1` identifica el commit inmediatamente anterior al actual.
- `--soft` mueve la referencia de la rama al commit anterior, pero conserva los cambios del commit eliminado en el staging.
- `git status` debe mostrar el cambio como preparado.
- Si quieres guardar otra vez ese cambio, ejecuta `git commit -m "Recuperar commit soft"`.

### 6.2 Probar `reset --mixed`

Primero crea un commit nuevo para que el ejercicio sea independiente:

```bash
git add README.md
git commit -m "Preparar prueba mixed"
echo "Cambio para mixed" >> README.md
git add README.md
git commit -m "Commit para mixed"
git reset --mixed HEAD~1
git status
```

- `--mixed` mueve la rama al commit anterior y quita los cambios del staging, pero mantiene las modificaciones en los archivos.
- Es el comportamiento predeterminado de `git reset`.
- Para volver a preparar el cambio, ejecuta `git add README.md`.

### 6.3 Probar `reset --hard`

Hazlo únicamente en este repositorio de prueba:

```bash
git add README.md
git commit -m "Preparar prueba hard"
echo "Cambio que se descartara" >> README.md
git add README.md
git commit -m "Commit que se descartara"
git reset --hard HEAD~1
git status
git log --oneline -4
```

- `--hard` mueve la rama al commit anterior y descarta los cambios del commit retirado del historial.
- También puede descartar cambios no confirmados en los archivos afectados.
- No uses este comando si necesitas conservar el trabajo.

## 7. Comprobación final

```bash
git status
git log --oneline
cat README.md
```

- `git status`: comprueba si quedan cambios pendientes.
- `git log --oneline`: enseña el historial que permanece en la rama.
- `cat README.md`: permite verificar el contenido del archivo.

## Resumen para memorizar

- `git add`: prepara cambios.
- `git commit`: guarda una versión.
- `git log`: consulta el historial.
- `git restore --staged archivo`: saca un archivo del staging.
- `git reset --soft HEAD~1`: quita el commit, mantiene los cambios preparados.
- `git reset --mixed HEAD~1`: quita el commit, mantiene los cambios sin preparar.
- `git reset --hard HEAD~1`: quita el commit y descarta sus cambios.
