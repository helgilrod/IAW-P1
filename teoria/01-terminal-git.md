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

## Las tres zonas de Git
| Zona | Qué hay | Comando que lo pasa a la siguiente |
|---|---|---|
| Directorio de trabajo | Los ficheros tal como los editas | git add |
| Staging (área de preparación) | Lo que irá en el próximo commit | git commit |
| Historial | Los commits guardados en tu ordenador | git push (los sube a GitHub) |

Como una foto: add coloca a la gente, commit hace la foto y push la envía.

## git add: elegir qué entra en el commit
~~~bash
git add notas.md
git add .
git add -A
git restore --staged notas.md
~~~
- git add notas.md prepara solo ese fichero.
- git add . prepara todo lo que ha cambiado en la carpeta actual y sus subcarpetas: ficheros nuevos, modificados y borrados. Deja fuera lo que diga el .gitignore.
- El punto significa «la carpeta en la que estoy»: dentro de una subcarpeta, solo añade esa. git add -A añade todo el repositorio, estés donde estés.
- git add no guarda nada en el historial (eso lo hace commit) ni sube nada a GitHub (eso lo hace push).
- Si el enunciado pide un commit con un solo fichero, nómbralo en vez de usar el punto.
- git restore --staged notas.md lo saca del staging sin tocar el fichero.

## Ciclo de trabajo
~~~bash
git status
git add .
git status
git diff --staged
git commit -m "Afegit fitxer Readme"
git log --oneline
~~~
- git status: en rojo lo que ha cambiado y no está preparado; en verde lo que entra en el commit. Míralo antes y después de git add.
- git diff muestra los cambios no preparados; git diff --staged, los preparados.
- git commit -m guarda lo preparado en el historial con ese mensaje.
- Descartar cambios locales sin commit: git restore README.md. Los cambios se pierden.

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
remote -v muestra remotos; remote add origin registra uno; push -u publica main y configura seguimiento; clone descarga repositorio; pull descarga e integra cambios; push sube commits. Paso a paso, en [Repositorios de cero](03-repos-de-cero.md).

## Errores típicos
| Mensaje | Causa | Solución |
|---|---|---|
| no changes added to commit | Has hecho commit sin git add | git add y vuelve a hacer el commit |
| Author identity unknown | Falta tu nombre o tu correo | git config --global user.name y user.email |
| not a git repository | No estás en la carpeta del repositorio | cd a la carpeta; ls -a debe mostrar .git |
| Ha entrado un fichero que no tocaba | git add . lo mete todo | git restore --staged fichero antes del commit |

Comprueba la rama antes del push y no uses reset --hard sin revisar antes git status.

## Comprueba que sabes
- [ ] Explicar las tres zonas y qué comando pasa de una a otra.
- [ ] Preparar un solo fichero y, después, todos con git add .
- [ ] Sacar un fichero del staging sin perder los cambios.
- [ ] Ver el historial con hash, autor y fecha.
- [ ] Explicar la diferencia entre reset --soft, --mixed y --hard.

---
[Índice](../README.md) · [02 .gitignore →](02-gitignore.md)
