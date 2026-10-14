# Práctica 2. GitHub Pages personal

## Paso 1. Crear repositorio
En GitHub crea el repositorio público con nombre exacto TU_USUARIO.github.io. Sustituye TU_USUARIO por tu usuario real. Si se proporciona un ZIP, descomprímelo en una carpeta.

## Paso 2. Comprobar archivos
~~~bash
cd carpeta-del-sitio
ls -la
~~~
Asegúrate de que index.html está en la raíz de lo que subirás.

## Paso 3. Inicializar y subir
Si la carpeta aún no es un repositorio:
~~~bash
git init
git branch -M main
git add .
git commit -m "Publicar sitio inicial"
git remote add origin https://github.com/TU_USUARIO/TU_USUARIO.github.io.git
git push -u origin main
~~~
Si ya clonaste el repo, no repitas init ni remote add; usa add, commit y push.

## Paso 4. Activar Pages
En GitHub abre Settings → Pages. Selecciona Deploy from a branch, rama main y carpeta /(root); guarda.

## Paso 5. Verificar
Abre https://TU_USUARIO.github.io/ y comprueba enlaces, CSS, imágenes y JavaScript.

## Si falla
Comprueba el nombre del repositorio, ubicación de index.html, rama publicada y estado en Settings → Pages.