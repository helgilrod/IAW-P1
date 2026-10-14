# Terminal y Git

## Crear e inspeccionar repositorio

```bash
mkdir proyecto
cd proyecto
ls -la
git init
git status
```

- `mkdir proyecto`: crea la carpeta `proyecto`.
- `cd proyecto`: entra en esa carpeta.
- `ls -la`: muestra archivos normales y ocultos, incluido `.git`.
- `git init`: inicializa un repositorio Git en la carpeta actual.
- `git status`: muestra la rama actual y el estado de los archivos.

## Configuración

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
git config --global core.editor nano
git config --global --list
```

- `git config --global user.name "Tu Nombre"`: configura el nombre que aparecerá como autor de los commits.
- `git config --global user.email "tu@email.com"`: configura el correo asociado a los commits.
- `git config --global core.editor nano`: establece Nano como editor predeterminado de Git.
- `git config --global --list`: muestra la configuración global guardada.

Sustituye los datos de ejemplo por los que debas usar en clase.

## Ciclo de trabajo

```bash
git add README.md
git diff
git diff --staged
git commit -m "Afegit fitxer Readme"
git log --oneline
```

- `git add README.md`: prepara el archivo para incluirlo en el siguiente commit.
- `git diff`: muestra cambios del directorio de trabajo que todavía no están preparados.
- `git diff --staged`: muestra los cambios que sí están preparados para el commit.
- `git commit -m "Afegit fitxer Readme"`: crea un commit con los cambios preparados y el mensaje indicado.
- `git log --oneline`: muestra el historial con un commit por línea.
- `git restore --staged README.md`: saca el archivo del staging sin borrarlo.
- `git restore README.md`: descarta cambios locales no confirmados de ese archivo; puede hacer perder trabajo.

Flujo habitual: editar → preparar con `git add` → comprobar con `git diff --staged` → confirmar con `git commit`.

## Historial

```bash
git log
git log --oneline
git log --pretty=format:"%h %an %ad" --date=iso
```

- `git log`: muestra el historial detallado.
- `git log --oneline`: resume cada commit en una línea.
- `git log --pretty=format:"%h %an %ad" --date=iso`: muestra el hash abreviado, autor y fecha en formato ISO.
- `%h`: representa el hash abreviado.
- `%H`: representa el hash completo.
- `%an`: representa el nombre del autor.
- `%ae`: representa el correo del autor.
- `%ad`: representa la fecha del autor.
- `%s`: representa el asunto del commit.

## Deshacer commits

```bash
git reset --soft HEAD~1
git reset --mixed HEAD~1
git reset --hard HEAD~1
```

- `HEAD~1`: identifica el commit anterior al actual.
- `git reset --soft HEAD~1`: elimina el último commit de la rama, pero conserva sus cambios en staging.
- `git reset --mixed HEAD~1`: elimina el último commit y conserva los cambios en el directorio de trabajo, fuera del staging.
- `git reset --hard HEAD~1`: elimina el último commit y descarta los cambios afectados del staging y del directorio de trabajo. Es destructivo; úsalo solo cuando estés seguro.

## Remotos

```bash
git remote -v
git remote add origin URL_DEL_REPOSITORIO
git branch -M main
git push -u origin main
git clone URL_DEL_REPOSITORIO
git pull
git push
```

- `git remote -v`: muestra los repositorios remotos configurados y sus URL.
- `git remote add origin URL_DEL_REPOSITORIO`: registra un remoto llamado `origin`; sustituye la URL de ejemplo por la del repositorio real.
- `git branch -M main`: cambia el nombre de la rama actual a `main`.
- `git push -u origin main`: sube la rama `main` a `origin` y configura el seguimiento para futuros `push` y `pull`.
- `git clone URL_DEL_REPOSITORIO`: descarga el repositorio remoto y crea una carpeta de trabajo con el historial.
- `git pull`: descarga e integra cambios del remoto de seguimiento.
- `git push`: sube los commits locales al remoto configurado para la rama actual.

## Errores típicos

- Comprueba `git status` antes de confirmar o subir cambios.
- Ejecuta `git branch --show-current` para verificar la rama.
- No confundas el directorio de trabajo, el staging y los commits.
- No uses `git reset --hard` si necesitas conservar cambios locales.
- No repitas `git remote add origin` si el remoto ya existe; compruébalo con `git remote -v`.
