# Práctica 2. Publicar una web personal con GitHub Pages

## Objetivo
Subir una página estática a GitHub y publicarla con una dirección del tipo `https://TU_USUARIO.github.io/`.

## 1. Crear el repositorio correcto
En GitHub crea un repositorio público llamado exactamente `TU_USUARIO.github.io`, sustituyendo el texto por tu nombre de usuario. Este formato identifica un sitio personal. Si la práctica incluye un ZIP, descomprímelo primero en una carpeta de trabajo.

## 2. Revisar la estructura
```bash
cd carpeta-del-sitio
ls -la
```
El archivo principal debe llamarse `index.html` y estar en la raíz que se publicará. Comprueba también que las rutas de CSS, imágenes y JavaScript sean relativas y que esos archivos existan.

## 3. Inicializar Git y subir el proyecto
Solo si la carpeta todavía no es un repositorio:
```bash
git init
git branch -M main
git add .
git status
git commit -m "Publicar sitio inicial"
git remote add origin https://github.com/TU_USUARIO/TU_USUARIO.github.io.git
git push -u origin main
```
Antes del commit, `git status` permite revisar qué se va a subir. Si has clonado el repositorio o ya tiene remoto, **no repitas** `git init` ni `git remote add origin`; añade, confirma y sube los cambios con normalidad.

## 4. Activar Pages
En el repositorio abre **Settings → Pages**. En la opción de origen selecciona **Deploy from a branch**, elige `main` y la carpeta `/(root)`, y guarda. GitHub procesará los archivos y publicará el sitio.

## 5. Verificar la web
Abre `https://TU_USUARIO.github.io/`. Comprueba:
- que carga `index.html`;
- que se ven los estilos;
- que las imágenes no aparecen rotas;
- que los enlaces funcionan;
- que la web se ve correctamente al cambiar el tamaño de la ventana.

## Errores frecuentes
Si no aparece la web, revisa el nombre exacto del repositorio, la rama y carpeta seleccionadas en Pages y el estado del despliegue. Si aparece HTML sin diseño, comprueba la ruta del CSS. Los cambios pueden tardar un poco en publicarse.