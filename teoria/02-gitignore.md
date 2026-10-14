# Archivo .gitignore

## Función
.gitignore indica qué archivos o rutas debe ignorar Git si aún no están bajo seguimiento. Se coloca normalmente en la raíz.

~~~gitignore
# Python
__pycache__/
*.py[cod]
venv/
.venv/
# Compilación
dist/
build/
*.exe
# Node
node_modules/
# Configuración privada
.env
# Sistema y temporales
.DS_Store
*.tmp
*.log
~~~

## Patrones
- *.log: archivos terminados en .log.
- *.exe: archivos terminados en .exe.
- carpeta/: directorio y contenido.
- /dist/: dist en la raíz.
- !importante.log: excepción que vuelve a incluir un patrón.
- # comentario: comentario explicativo.

Cada patrón va en una línea. El asterisco coincide con una secuencia de caracteres.

## Comprobar
~~~bash
git status
git add .gitignore
git add .
git status
~~~

Importante: .gitignore no deja de seguir un archivo ya controlado. Para quitarlo del índice sin borrarlo del disco: git rm --cached archivo; después crea un commit. No guardes contraseñas, tokens ni claves privadas en el repositorio.