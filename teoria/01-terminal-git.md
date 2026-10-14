# Terminal y Git

## 1. Terminal: moverse y trabajar con archivos

```bash
pwd
ls
ls -la
cd carpeta
cd ..
cd ~
mkdir proyecto
mkdir -p carpeta/subcarpeta
touch archivo.txt
cat archivo.txt
cp origen.txt copia.txt
mv archivo.txt carpeta/
rm archivo.txt
```

- `pwd`: muestra la ruta de la carpeta actual.
- `ls`: lista los archivos y carpetas visibles.
- `ls -la`: lista también los archivos ocultos.
- `cd carpeta`: entra en una carpeta.
- `cd ..`: sube a la carpeta superior.
- `cd ~`: vuelve a la carpeta personal del usuario.
- `mkdir proyecto`: crea una carpeta.
- `mkdir -p carpeta/subcarpeta`: crea toda la ruta si todavía no existe.
- `touch archivo.txt`: crea un archivo vacío o actualiza su fecha.
- `cat archivo.txt`: muestra el contenido de un archivo de texto.
- `cp origen.txt copia.txt`: copia un archivo.
- `mv archivo.txt carpeta/`: mueve o cambia el nombre de un archivo.
- `rm archivo.txt`: elimina un archivo; no suele enviarlo a la papelera.
- `find . -maxdepth 3 -type f`: lista archivos hasta tres niveles por debajo de la carpeta actual.

## 2. Crear e inspeccionar un repositorio Git

```bash
mkdir proyecto
cd proyecto
git init
git status
git branch -M main
```

- `git init`: inicializa un repositorio Git en la carpeta actual.
- `git status`: muestra la rama actual y el estado de los archivos.
- `git branch -M main`: cambia el nombre de la rama actual a `main`; no crea un repositorio remoto.

## 3. Configuración de Git

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
git config --global core.editor nano
git config --global --list
git config user.name
```

- `git config --global user.name "Tu Nombre"`: configura el nombre del autor de los commits.
- `git config --global user.email "tu@email.com"`: configura el correo asociado a los commits.
- `git config --global core.editor nano`: establece Nano como editor predeterminado de Git.
- `git config --global --list`: muestra la configuración global.
- `git config user.name`: consulta el nombre configurado para el repositorio actual.
- Sustituye los datos de ejemplo por los que debas usar en clase.

## 4. Ciclo de trabajo: cambios, staging y commits

```bash
git status
git add archivo.txt
git add .
git diff
git diff --staged
git commit -m "Añadir archivo"
git status
```

- `git status`: muestra archivos nuevos, modificados, preparados y la rama actual.
- `git add archivo.txt`: prepara un archivo para el próximo commit.
- `git add .`: prepara los cambios dentro de la carpeta actual y sus subcarpetas; revisa antes que no incluya archivos que no debas subir.
- `git diff`: muestra cambios del directorio de trabajo que aún no están preparados.
- `git diff --staged`: muestra los cambios preparados para el próximo commit.
- `git commit -m "Añadir archivo"`: guarda un commit con los cambios preparados.
- `git status` después del commit: comprueba si quedan cambios pendientes.

Flujo habitual: **editar → `git add` → revisar con `git diff --staged` → `git commit`**.

## 5. Deshacer o corregir cambios

### 5.1 Retirar cambios del staging o del archivo

```bash
git restore --staged archivo.txt
git restore archivo.txt
```

- `git restore --staged archivo.txt`: saca el archivo del staging, pero conserva los cambios en el archivo.
- `git restore archivo.txt`: descarta los cambios locales no confirmados de ese archivo y recupera la versión del último commit. Puede hacer perder trabajo.

### 5.2 Deshacer el último commit con reset

```bash
git reset --soft HEAD~1
git reset --mixed HEAD~1
git reset --hard HEAD~1
```

- `HEAD`: representa el commit actual.
- `HEAD~1`: representa el commit anterior al actual.
- `git reset --soft HEAD~1`: mueve la rama al commit anterior, pero conserva los cambios preparados en staging.
- `git reset --mixed HEAD~1`: mueve la rama al commit anterior y deja los cambios en el directorio de trabajo, fuera del staging. Es la opción predeterminada de `git reset`.
- `git reset --hard HEAD~1`: mueve la rama al commit anterior y descarta los cambios afectados del staging y del directorio de trabajo. **Es destructivo**.

Usa `reset` con cuidado, especialmente si los commits ya se han compartido con otras personas.

### 5.3 Deshacer un commit con revert

```bash
git revert HASH_DEL_COMMIT
git log --oneline
```

- `git revert HASH_DEL_COMMIT`: crea un nuevo commit que revierte los cambios de un commit anterior; no borra el historial existente.
- Sustituye `HASH_DEL_COMMIT` por el hash real que aparece en `git log`.
- Si se abre un editor, guarda y cierra el mensaje del commit para completar el revert.
- Si aparecen conflictos, resuélvelos, prepara los archivos con `git add` y termina el proceso según indique Git.
- Es normalmente más seguro que `reset` para deshacer cambios que ya se han compartido.

### 5.4 Recuperar o inspeccionar referencias anteriores

```bash
git reflog
git show HASH_DEL_COMMIT
```

- `git reflog`: muestra movimientos recientes de `HEAD` y puede ayudar a localizar un commit después de un reset.
- `git show HASH_DEL_COMMIT`: muestra los detalles y cambios de un commit concreto.
- Sustituye el hash de ejemplo por el real. No ejecutes un reset de recuperación sin identificar antes el commit correcto.

## 6. Historial y ramas

Las ramas permiten trabajar en líneas de desarrollo separadas. Los comandos de ramas se estudian junto con el historial porque el historial permite ver dónde está cada rama y cómo se integraron los cambios.

### 6.1 Consultar el historial

```bash
git log
git log --oneline
git log --oneline --graph --all
git log --oneline -5
git show HASH_DEL_COMMIT
git rev-list --count HEAD
```

- `git log`: muestra el historial detallado de commits.
- `git log --oneline`: resume cada commit en una línea.
- `git log --oneline --graph --all`: muestra el historial de todas las ramas con un gráfico de líneas y uniones.
- `git log --oneline -5`: muestra los cinco commits más recientes.
- `git show HASH_DEL_COMMIT`: muestra los detalles de un commit y los cambios que contiene.
- `git rev-list --count HEAD`: cuenta los commits alcanzables desde el commit actual.

Formatos personalizados para `git log`:

```bash
git log --pretty=format:"%h %an %ad %s" --date=iso
```

- `%h`: hash abreviado.
- `%H`: hash completo.
- `%an`: nombre del autor.
- `%ae`: correo del autor.
- `%ad`: fecha del autor.
- `%s`: asunto del commit.
- `--date=iso`: muestra la fecha en formato ISO.

### 6.2 Ver y crear ramas

```bash
git branch
git branch -a
git branch nombre-rama
git switch -c nombre-rama
git branch --show-current
```

- `git branch`: lista las ramas locales; el asterisco identifica la rama actual.
- `git branch -a`: lista las ramas locales y las ramas remotas conocidas.
- `git branch nombre-rama`: crea una rama, pero no cambia a ella.
- `git switch -c nombre-rama`: crea una rama y cambia a ella en un solo comando.
- `git branch --show-current`: muestra el nombre de la rama actual.

### 6.3 Cambiar de rama

```bash
git switch main
git switch nombre-rama
git checkout main
git checkout -b nombre-rama
```

- `git switch main`: cambia a la rama existente `main`.
- `git switch nombre-rama`: cambia a una rama existente.
- `git checkout main`: forma antigua, todavía válida, para cambiar a `main`.
- `git checkout -b nombre-rama`: forma antigua de crear una rama y cambiar a ella.
- Antes de cambiar de rama, revisa `git status`; Git puede impedir el cambio si hay cambios que se sobrescribirían.

### 6.4 Integrar una rama con merge

```bash
git switch main
git merge nombre-rama
git log --oneline --graph --all
```

- `git switch main`: cambia a la rama que recibirá los cambios.
- `git merge nombre-rama`: integra en la rama actual los cambios de `nombre-rama`.
- `git log --oneline --graph --all`: permite comprobar el resultado y ver cómo se relacionan las ramas.

Si aparece un conflicto:
1. Ejecuta `git status` para localizar los archivos en conflicto.
2. Edita cada archivo y decide qué contenido conservar.
3. Elimina los marcadores de conflicto `<<<<<<<`, `=======` y `>>>>>>>`.
4. Ejecuta `git add archivo-resuelto`.
5. Completa el merge con `git commit` si Git no lo ha terminado automáticamente.

### 6.5 Eliminar una rama

```bash
git branch -d nombre-rama
git branch -D nombre-rama
```

- `git branch -d nombre-rama`: elimina una rama local que ya se ha integrado de forma segura.
- `git branch -D nombre-rama`: fuerza la eliminación de la rama local incluso si tiene cambios no integrados; puede perder trabajo.
- Para eliminar una rama remota: `git push origin --delete nombre-rama`. Comprueba primero que sea la rama correcta.

## 7. Remotos: conectar, descargar y subir

```bash
git remote -v
git remote add origin URL_DEL_REPOSITORIO
git remote set-url origin URL_DEL_REPOSITORIO
git clone URL_DEL_REPOSITORIO
git fetch
git pull
git push -u origin main
git push
```

- `git remote -v`: muestra los repositorios remotos configurados y sus URL.
- `git remote add origin URL_DEL_REPOSITORIO`: registra un remoto llamado `origin`; sustituye la URL por la real.
- `git remote set-url origin URL_DEL_REPOSITORIO`: cambia la URL de un remoto que ya existe.
- `git clone URL_DEL_REPOSITORIO`: descarga el repositorio remoto y crea una carpeta de trabajo con su historial.
- `git fetch`: descarga información y commits remotos sin integrarlos automáticamente en la rama actual.
- `git pull`: descarga e integra cambios del remoto seguido por la rama actual.
- `git push -u origin main`: sube `main` a `origin` y configura el seguimiento para futuros comandos.
- `git push`: sube los commits de la rama actual al remoto configurado.

Si aparece `remote origin already exists`, consulta `git remote -v` o corrige la URL con `git remote set-url`; no repitas `git remote add`.

## 8. Etiquetas (tags)

```bash
git tag
git tag -a v1.0 -m "Version 1.0"
git show v1.0
git push origin v1.0
git push origin --tags
```

- `git tag`: lista las etiquetas locales.
- `git tag -a v1.0 -m "Version 1.0"`: crea una etiqueta anotada para marcar una versión.
- `git show v1.0`: muestra información de la etiqueta y el commit al que apunta.
- `git push origin v1.0`: sube una etiqueta concreta al remoto.
- `git push origin --tags`: sube las etiquetas locales al remoto.

## 9. Errores típicos y comprobaciones

- Ejecuta `git status` antes de hacer commit, cambiar de rama o subir cambios.
- Usa `git branch --show-current` para comprobar en qué rama estás.
- Usa `git log --oneline --graph --all` para visualizar el historial y las ramas.
- No confundas el directorio de trabajo, el staging y los commits.
- `restore` sirve para descartar o retirar cambios de archivos; `reset` mueve la rama a otro commit; `revert` crea un nuevo commit que revierte otro.
- No uses `git reset --hard` ni `git branch -D` si necesitas conservar el trabajo.
- No repitas `git remote add origin` si el remoto ya existe.
