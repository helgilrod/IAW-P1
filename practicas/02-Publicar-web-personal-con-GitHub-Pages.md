# Práctica 2. Crear una web, subir una foto y publicarla con GitHub Pages

## Objetivo
Crear una web HTML, guardar una imagen dentro del proyecto, mostrarla con la etiqueta `img`, subir los archivos a GitHub y publicar la página con GitHub Pages.

> Si el enunciado ya proporciona HTML, CSS, imágenes o nombres concretos, utiliza esos archivos y nombres. Los siguientes comandos son un ejemplo completo para practicar desde cero.

## 1. Crear el repositorio en GitHub

1. Entra en GitHub y pulsa **New repository**.
2. Para una web personal con URL `https://TU_USUARIO.github.io/`, el repositorio debe llamarse exactamente `TU_USUARIO.github.io`.
3. Sustituye `TU_USUARIO` por tu nombre de usuario real.
4. Si el enunciado pide un sitio de proyecto en lugar de una web personal, usa el nombre del repositorio indicado por el profesor.
5. Crea el repositorio. Copia su URL HTTPS.

## 2. Preparar la carpeta local

Si empiezas desde cero, crea la carpeta y entra en ella:

```bash
mkdir mi-web
cd mi-web
pwd
```

- `mkdir mi-web`: crea la carpeta del proyecto.
- `cd mi-web`: entra en la carpeta.
- `pwd`: comprueba dónde estás trabajando.

Crea la carpeta de imágenes:

```bash
mkdir -p assets/images
find . -maxdepth 3 -type d
```

- `mkdir -p assets/images`: crea la ruta donde se guardará la foto.
- `find ... -type d`: muestra las carpetas creadas.

## 3. Copiar la foto al proyecto

Copia tu foto dentro de `assets/images/` con el explorador de archivos o desde la terminal. Por ejemplo, si la foto está en Descargas y se llama `foto.jpg`:

```bash
cp ~/Descargas/foto.jpg assets/images/foto.jpg
ls -lh assets/images/
file assets/images/foto.jpg
```

- `cp origen destino`: copia la imagen a la carpeta del proyecto. Cambia la ruta de origen por la ubicación real del archivo.
- `ls -lh assets/images/`: comprueba que la imagen está ahí y muestra su tamaño.
- `file ...`: identifica el tipo de archivo.
- Respeta las mayúsculas y minúsculas del nombre: `Foto.jpg` y `foto.jpg` pueden ser archivos distintos.

Si tu foto se llama `imagen.png`, usa ese nombre en los siguientes pasos. No hace falta que se llame literalmente `foto.jpg`.

## 4. Crear el HTML e insertar la foto

Crea `index.html` en la raíz del proyecto:

```bash
cat > index.html <<'EOF'
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Mi web personal</title>
</head>
<body>
  <h1>Mi web personal</h1>
  <p>Esta es mi primera página web.</p>
  <h2>Mi foto</h2>
  <img src="assets/images/foto.jpg" alt="Foto personal" width="400">
</body>
</html>
EOF
cat index.html
```

- `cat > index.html`: crea el archivo con el contenido indicado.
- `<h1>`: título principal de la página.
- `<img>`: inserta una imagen en la página.
- `src="assets/images/foto.jpg"`: ruta relativa desde `index.html` hasta la imagen.
- `alt="Foto personal"`: texto alternativo si la imagen no se puede ver y apoyo a la accesibilidad.
- `width="400"`: limita el ancho visual de la imagen a 400 píxeles.
- El nombre de la ruta del `src` debe coincidir exactamente con la carpeta y el archivo que has subido.

Si la imagen se llama de otra manera, modifica el valor de `src`. Por ejemplo, para `assets/images/imagen.png`, escribe `src="assets/images/imagen.png"`.

## 5. Comprobar la estructura

```bash
find . -maxdepth 4 -type f
```

La estructura debe parecerse a esta:

```text
mi-web/
├── index.html
└── assets/
    └── images/
        └── foto.jpg
```

- `index.html` debe estar en la raíz que se publicará.
- La foto debe estar dentro de `assets/images/`.
- No escribas rutas del ordenador como `/home/usuario/Descargas/foto.jpg` en el HTML; la web publicada no podrá acceder a esa ruta local.

## 6. Probar la web antes de subirla

Abre `index.html` en el navegador. En Linux puedes ejecutar:

```bash
xdg-open index.html
```

- `xdg-open index.html`: abre el archivo con la aplicación predeterminada.
- Comprueba que aparece el título y que se ve la foto.
- Si la foto está rota, comprueba el nombre, la extensión, las mayúsculas y el valor de `src`.

## 7. Inicializar Git y hacer el primer commit

Ejecuta estos comandos solo si la carpeta aún no es un repositorio Git:

```bash
git init
git branch -M main
git status
git add index.html assets/images/foto.jpg
git status
git diff --staged
git commit -m "Crear web personal con foto"
```

- `git init`: crea el repositorio local.
- `git branch -M main`: establece el nombre de la rama.
- Primer `git status`: muestra los archivos detectados.
- `git add ...`: prepara el HTML y la foto para el commit. Cambia la ruta si la imagen tiene otro nombre.
- Segundo `git status`: comprueba qué archivos se van a guardar.
- `git diff --staged`: revisa los cambios preparados en archivos de texto. Las imágenes no se muestran como texto legible.
- `git commit -m "..."`: registra los archivos en el historial.

## 8. Conectar con GitHub y subir los archivos

Copia la URL HTTPS del repositorio creado y úsala en lugar de la URL de ejemplo:

```bash
git remote add origin https://github.com/TU_USUARIO/TU_USUARIO.github.io.git
git remote -v
git push -u origin main
```

- `git remote add origin ...`: conecta el repositorio local con el remoto de GitHub.
- `git remote -v`: comprueba las direcciones del remoto.
- `git push -u origin main`: sube la rama `main) y establece el seguimiento remoto.

Si aparece `remote origin already exists`, no repitas `git remote add`; comprueba la dirección con `git remote -v`. Si ya habías creado el repositorio local o lo clonaste, no vuelvas a ejecutar `git init`.

## 9. Activar GitHub Pages

Para un repositorio personal `TU_USUARIO.github.io`, normalmente se publica desde la raíz de `main`:

1. Abre el repositorio en GitHub.
2. Entra en **Settings → Pages**.
3. En **Build and deployment**, selecciona **Deploy from a branch**.
4. Selecciona la rama `main` y la carpeta `/(root)`.
5. Pulsa **Save**.
6. Espera a que termine el despliegue.

Si el enunciado pide una web de proyecto, la URL normalmente tiene la forma `https://TU_USUARIO.github.io/NOMBRE_REPOSITORIO/`; sigue la configuración que indique el profesor.

## 10. Verificar la web publicada

Abre la URL que muestre GitHub Pages. Comprueba:
- que se carga el HTML;
- que se ve la fotografía;
- que no hay rutas rotas;
- que el diseño funciona en una ventana estrecha y una ancha.

## 11. Actualizar la web después de cambiar la foto o el HTML

Cada vez que modifiques el HTML o sustituyas la imagen:

```bash
git status
git add index.html assets/images/
git diff --staged
git commit -m "Actualizar web y fotografia"
git push
```

- `git status`: detecta los archivos modificados y nuevos.
- `git add index.html assets/images/`: prepara el HTML y los cambios de la carpeta de imágenes.
- `git diff --staged`: revisa los cambios de texto preparados.
- `git commit`: guarda la nueva versión.
- `git push`: sube los cambios; Pages desplegará la nueva versión automáticamente.

## Errores frecuentes

- **La foto se ve localmente, pero no online:** comprueba que la imagen está subida a GitHub y que la ruta es relativa.
- **La imagen aparece rota:** revisa el nombre exacto, extensión y mayúsculas/minúsculas.
- **No aparece la web:** comprueba que Pages publica la rama y carpeta correctas y que existe `index.html`.
- **La URL no coincide con la esperada:** distingue entre web personal `TU_USUARIO.github.io` y web de proyecto.
- **Git rechaza el push:** revisa que el remoto sea correcto y que tengas permisos de escritura.
