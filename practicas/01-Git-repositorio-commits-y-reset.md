# Práctica 1. Crear un repositorio Git y trabajar con commits

## Objetivo
Crear un repositorio, preparar archivos, registrar commits, consultar el historial y comparar `reset --soft`, `reset --mixed` y `reset --hard`.

## 1. Crear la carpeta e inicializar Git

```bash
mkdir activitatsPrimeraClaseDAW
cd activitatsPrimeraClaseDAW
git init
git branch -M main
ls -la
git status
```

- `mkdir activitatsPrimeraClaseDAW`: crea la carpeta de la práctica.
- `cd activitatsPrimeraClaseDAW`: entra en la carpeta.
- `git init`: inicializa el repositorio local dentro de `.git/`.
- `git branch -M main`: cambia el nombre de la rama actual a `main`.
- `ls -la`: muestra los archivos, incluidos los ocultos.
- `git status`: muestra la rama y el estado del repositorio.

## 2. Configurar la identidad
Hazlo solo si Git aún no conoce tu identidad:

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
git config --global core.editor nano
git config --global --list
```

- `git config --global user.name "Tu Nombre"`: configura el nombre del autor.
- `git config --global user.email "tu@email.com"`: configura el correo del autor.
- `git config --global core.editor nano`: selecciona Nano como editor predeterminado.
- `git config --global --list`: comprueba la configuración guardada.

Sustituye los valores de ejemplo por los que indique el profesor.

## 3. Crear y revisar un archivo

```bash
echo "# Activitats primera clase DAW" > README.md
cat README.md
git status
```

- `echo "..." > README.md`: crea el archivo y escribe el título. `>` sobrescribe si ya existe.
- `cat README.md`: muestra su contenido.
- `git status`: comprueba que el archivo todavía no está bajo seguimiento.

## 4. Entender el staging

```bash
git add README.md
git status
git restore --staged README.md
git status
```

- Primer `git add README.md`: prepara el archivo para el próximo commit.
- Primer `git status`: muestra el archivo en staging.
- `git restore --staged README.md`: lo saca del staging sin borrarlo.
- Segundo `git status`: confirma que ya no está preparado.

## 5. Guardar un commit y leer el historial

```bash
git add README.md
git diff --staged
git commit -m "Afegit fitxer Readme"
git log --oneline
git log --pretty=format:"%h %an %ad" --date=iso
```

- `git add README.md`: prepara el archivo.
- `git diff --staged`: revisa los cambios que se incluirán.
- `git commit -m "Afegit fitxer Readme"`: crea el commit.
- `git log --oneline`: muestra el historial resumido.
- `git log --pretty=format:"%h %an %ad" --date=iso`: muestra hash abreviado, autor y fecha.

## 6. Comparar los tres reset
Primero crea un cambio de prueba y guárdalo en un commit:

```bash
echo "Línia de prova" >> README.md
cat README.md
git add README.md
git commit -m "Prova de canvi"
git log --oneline -3
```

- `echo "Línia de prova" >> README.md`: añade una línea al final del archivo sin borrar el contenido anterior.
- `cat README.md`: comprueba la línea añadida.
- `git add README.md`: prepara el cambio.
- `git commit -m "Prova de canvi"`: guarda el cambio en un commit.
- `git log --oneline -3`: muestra los tres commits más recientes.

Ahora prueba cada modalidad **por separado** y ejecuta `git status` después de cada una. Para comparar las tres sobre un cambio similar, vuelve a crear la línea y el commit de prueba entre ejercicios.

- `git reset --soft HEAD~1`: elimina el último commit, pero deja sus cambios preparados en staging.
- `git reset --mixed HEAD~1`: elimina el último commit y deja sus cambios en el directorio de trabajo, fuera del staging. Es el comportamiento predeterminado.
- `git reset --hard HEAD~1`: elimina el último commit y descarta los cambios afectados. Úsalo solo en este repositorio de prueba porque puede destruir trabajo.

## Comprobación final

```bash
git status
git log --oneline
cat README.md
```

- `git status`: muestra si quedan cambios sin confirmar.
- `git log --oneline`: muestra los commits que siguen en el historial.
- `cat README.md`: permite comprobar el contenido final del archivo.
