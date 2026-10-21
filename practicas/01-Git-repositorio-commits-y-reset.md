# Práctica 1. Crear un repositorio Git y trabajar con commits

## Qué se aprende
Git guarda versiones del proyecto. En esta práctica se crea un repositorio, se prepara un archivo para guardarlo, se registra un commit, se consulta el historial y se compara qué hacen `reset --soft`, `--mixed` y `--hard`.

## 1. Crear la carpeta y activar Git
```bash
mkdir activitatsPrimeraClaseDAW
cd activitatsPrimeraClaseDAW
git init
git branch -M main
ls -la
```
- `mkdir` crea la carpeta.
- `cd` entra en ella.
- `git init` crea el repositorio local, dentro de `.git/`.
- `git branch -M main` establece `main` como nombre de la rama actual.
- `ls -la` permite ver también archivos ocultos.

## 2. Configurar la identidad
Hazlo solo si Git aún no conoce tu identidad:
```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
git config --global core.editor nano
```
El nombre y correo se guardan como autoría de los commits. Sustituye los ejemplos por los datos que debas utilizar en clase.

## 3. Crear y revisar un archivo
```bash
echo "# Activitats primera clase DAW" > README.md
cat README.md
git status
```
El operador `>` crea o sobrescribe el archivo. `git status` muestra si hay archivos sin seguimiento o cambios pendientes.

## 4. Entender el staging
```bash
git add README.md
git status
git restore --staged README.md
git status
```
`git add` prepara los cambios para el próximo commit. `git restore --staged` los saca del staging, pero **no borra el archivo**. El flujo habitual es: editar → preparar → confirmar.

## 5. Guardar un commit y leer el historial
```bash
git add README.md
git commit -m "Afegit fitxer Readme"
git log --oneline
git log --pretty=format:"%h %an %ad" --date=iso
```
Un commit es una versión identificada por un hash. El mensaje debe describir el cambio. El segundo comando de log muestra hash abreviado, autor y fecha.

## 6. Comparar los tres reset
Primero crea un cambio de prueba y un commit:
```bash
echo "Línia de prova" >> README.md
git add README.md
git commit -m "Prova de canvi"
```
Ahora prueba cada modalidad por separado, comprobando `git status` después de cada una. Si quieres observar los tres resultados, recrea el commit de prueba entre ejercicios.
- `git reset --soft HEAD~1`: quita el último commit, conserva los cambios en staging.
- `git reset --mixed HEAD~1`: quita el commit y deja los cambios en el directorio de trabajo, fuera del staging. Es el comportamiento predeterminado.
- `git reset --hard HEAD~1`: mueve la rama y descarta los cambios afectados. **Úsalo solo en este repositorio de prueba**: puede destruir trabajo.

## 7. Guardar el historial en un fichero
```bash
git log --oneline > historial.txt
cat historial.txt
git add historial.txt
git commit -m "Guardar historial"
```
`>` envía la salida del comando al fichero en vez de a la pantalla; es más seguro que copiar y pegar. El commit en el que añades `historial.txt` no aparece dentro, porque el fichero se creó antes.

## 8. Subir el repositorio a GitHub
1. En GitHub: **+** → **New repository**, con el mismo nombre que la carpeta, público y **sin** README (si GitHub crea un commit inicial, tu primer push se rechaza).
2. Copia la URL HTTPS del botón **Code** y conecta el repositorio local:
```bash
git remote add origin https://github.com/TU_USUARIO/activitatsPrimeraClaseDAW.git
git remote -v
git push -u origin main
```
- `-u` deja configurado el seguimiento; los siguientes envíos son solo `git push`.
- Como contraseña, GitHub pide un token personal, no la contraseña de la cuenta.
- Si sale `rejected ... (fetch first)`, el remoto tiene commits que tú no: `git pull --rebase origin main` y vuelve a hacer push.
- Más errores típicos y sus soluciones, en [Repositorios de cero](../teoria/06-repos-de-cero.md#errores-típicos).

## Comprobación final
Ejecuta `git status` y `git log --oneline`. Debes poder explicar qué está en el directorio de trabajo, qué está preparado y qué forma parte del historial.