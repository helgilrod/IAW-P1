# Práctica 1. Git inicial

## Objetivo
Crear un repositorio local, practicar staging, commits, log y reset.

## Paso 1. Crear repositorio
~~~bash
mkdir activitatsPrimeraClaseDAW
cd activitatsPrimeraClaseDAW
git init
ls -la
~~~
Si el ejercicio requiere main: git branch -M main.

## Paso 2. Configurar identidad si falta
~~~bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
git config --global core.editor nano
~~~

## Paso 3. Crear README y ver estado
~~~bash
echo "# Activitats primera clase DAW" > README.md
cat README.md
git status
~~~
Sustituye el texto por la descripción exacta del enunciado.

## Paso 4. Añadir y retirar del staging
~~~bash
git add README.md
git status
git restore --staged README.md
git status
~~~
El archivo sigue en disco, pero ya no está preparado.

## Paso 5. Crear commit e inspeccionar historial
~~~bash
git add README.md
git commit -m "Afegit fitxer Readme"
git log --oneline
git log --pretty=format:"%h %an %ad" --date=iso
~~~

## Paso 6. Reset soft
Haz un cambio, añádelo y confirma:
~~~bash
echo "Línia de prova" >> README.md
git add README.md
git commit -m "Prova de canvi"
git reset --soft HEAD~1
git status
~~~
El commit se retira y los cambios quedan en staging.

## Paso 7. Reset mixed
~~~bash
git reset --mixed HEAD~1
git status
~~~
Los cambios quedan en el directorio de trabajo, fuera del staging.

## Paso 8. Reset hard
Solo en un repositorio de práctica:
~~~bash
git reset --hard HEAD~1
git status
~~~
Descarta cambios. No lo uses si necesitas conservar trabajo.

## Resultado
Saber diferenciar directorio de trabajo, staging e historial; crear commits, leer el log y elegir el modo de reset correcto.