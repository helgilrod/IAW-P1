# Práctica 2. Publicar una web personal con GitHub Pages

## Objetivo
Subir una página estática a GitHub y publicarla con una dirección del tipo `https://TU_USUARIO.github.io/`.

**Teoría:** [Repositorios de cero](../teoria/03-repos-de-cero.md) · [Pages y despliegues](../teoria/08-pages-despliegues.md)

## 1. Crear el repositorio correcto
En GitHub crea un repositorio público llamado exactamente `TU_USUARIO.github.io`, sustituyendo el texto por tu nombre de usuario. Este formato identifica un sitio personal y solo puede haber uno por cuenta: si ya existe y quieres empezar de cero, bórralo en **Settings → General → Danger Zone → Delete this repository** y vuelve a crearlo. Si vas a subir los archivos desde la terminal, créalo **sin** README.

Si la práctica incluye un ZIP, descomprímelo primero en una carpeta de trabajo (`unzip archivo.zip`, o en Windows clic derecho → **Extraer todo**).

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

### Alternativa: subir desde la web
En el repositorio pulsa **Add file → Upload files** y arrastra el **contenido** de la carpeta (no el .zip). `index.html` tiene que quedar en la raíz del repositorio. Termina con **Commit changes**.

## 4. Activar Pages
En el repositorio abre **Settings → Pages**. En la opción de origen selecciona **Deploy from a branch**, elige `main` y la carpeta `/(root)`, y guarda. GitHub procesará los archivos y publicará el sitio. Tarda uno o dos minutos: el progreso se ve en la pestaña **Actions** («pages build and deployment») y la URL aparece arriba en **Settings → Pages**.

## 5. Verificar la web
Abre `https://TU_USUARIO.github.io/`. Comprueba:
- que carga `index.html`;
- que se ven los estilos;
- que las imágenes no aparecen rotas;
- que los enlaces funcionan;
- que la web se ve correctamente al cambiar el tamaño de la ventana.

## 6. Hacer cambios después en local
```bash
git clone https://github.com/TU_USUARIO/TU_USUARIO.github.io.git
cd TU_USUARIO.github.io
nano README.md
git add README.md
git commit -m "Actualizar README"
git push
```
Cada push a `main` vuelve a publicar la web. Si el repositorio ya está en tu ordenador, no lo clones otra vez: trae lo último con `git pull`.

## Errores frecuentes
Si no aparece la web, revisa el nombre exacto del repositorio, la rama y carpeta seleccionadas en Pages y el estado del despliegue. Si aparece HTML sin diseño, comprueba la ruta del CSS. Los cambios pueden tardar un poco en publicarse.

---
[← Práctica 1](01-Git-repositorio-commits-y-reset.md) · [Índice de prácticas](README.md) · [Práctica 3 →](03-Compilar-Python-con-PyInstaller-en-tres-sistemas.md)
