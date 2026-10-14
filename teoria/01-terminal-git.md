# Terminal y Git

## Crear e inspeccionar repositorio
~~~bash
mkdir proyecto
cd proyecto
ls -la
git init
git status
~~~
- mkdir crea una carpeta; cd entra en ella.
- ls -la muestra archivos ocultos, incluido .git.
- git init inicializa el repositorio.
- git status muestra cambios y staging.

## Configuración
~~~bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
git config --global core.editor nano
git config --global --list
~~~

## Ciclo de trabajo
~~~bash
git add README.md
git diff --staged
git commit -m "Afegit fitxer Readme"
git log --oneline
~~~
git add prepara cambios; git commit crea un commit; git diff muestra cambios no preparados; git diff --staged muestra cambios preparados.
Quitar del staging sin borrar: git restore --staged README.md.
Descartar cambios locales sin commit: git restore README.md (puede perder trabajo).

## Historial
~~~bash
git log
git log --oneline
git log --pretty=format:"%h %an %ad" --date=iso
~~~
%h hash abreviado; %H hash completo; %an autor; %ae correo; %ad fecha; %s asunto.

## Deshacer commits
~~~bash
git reset --soft HEAD~1
git reset --mixed HEAD~1
git reset --hard HEAD~1
~~~
- --soft elimina el commit y conserva cambios en staging.
- --mixed elimina el commit y deja cambios fuera del staging.
- --hard elimina el commit y descarta cambios del staging y directorio de trabajo. Es destructivo.
- HEAD~1 representa el commit anterior.

## Remotos
~~~bash
git remote -v
git remote add origin URL_DEL_REPOSITORIO
git branch -M main
git push -u origin main
git clone URL_DEL_REPOSITORIO
git pull
git push
~~~
remote -v muestra remotos; remote add origin registra uno; push -u publica main y configura seguimiento; clone descarga repositorio; pull descarga e integra cambios; push sube commits.

## Errores típicos
No olvidar git add; comprobar la rama antes del push; distinguir staging y directorio de trabajo; no usar reset --hard sin revisar cambios.