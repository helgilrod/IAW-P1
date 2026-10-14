# GitHub Pages y despliegues

GitHub Pages publica sitios estáticos con HTML, CSS y JavaScript del lado del cliente.

## Publicación desde una rama
Crea `index.html`, guárdalo y sube los cambios:

```bash
git status
git add .
git diff --staged
git commit -m "Crear página inicial"
git push
```

- `git status`: muestra los archivos nuevos o modificados.
- `git add .`: prepara los cambios de la carpeta actual.
- `git diff --staged`: permite revisar los cambios que se van a confirmar.
- `git commit -m "Crear página inicial"`: guarda los cambios preparados en el historial.
- `git push`: sube los commits al remoto configurado para la rama actual.

En GitHub:
1. Abre **Settings → Pages**.
2. Selecciona **Deploy from a branch**.
3. Selecciona la rama indicada (normalmente `main`).
4. Selecciona `/(root)` o `/docs`, según dónde esté `index.html).
5. Guarda y espera al despliegue.

## Nombres y URL

- Repositorio personal: `usuario.github.io` → `https://usuario.github.io/`.
- Repositorio de proyecto: `mi-proyecto` → `https://usuario.github.io/mi-proyecto/`.

## Publicar desde docs

Estructura de ejemplo:

```text
repositorio/
├── README.md
└── docs/
    ├── index.html
    ├── css/estilos.css
    └── js/script.js
```

- `docs/`: carpeta seleccionada como origen de publicación.
- `docs/index.html`: archivo principal que debe existir en esa carpeta.
- `docs/css/estilos.css`: ejemplo de hoja de estilos.
- `docs/js/script.js`: ejemplo de archivo JavaScript.

En **Settings → Pages**, selecciona la rama `main` y la carpeta `/docs`. La carpeta elegida debe contener `index.html`.

## Compilar y desplegar no son lo mismo

- **Checkout:** descarga el código del repositorio en el runner.
- **Instalar:** prepara dependencias, por ejemplo con `npm install`.
- **Build:** genera los archivos finales, por ejemplo con `npm run build`.
- **Test:** ejecuta pruebas, por ejemplo con `npm test`.
- **Artifact:** conserva archivos generados para descargarlos después.
- **Deploy:** publica la aplicación en el destino correspondiente.

En React, la salida suele ser `build/` o `dist/`, según la herramienta. El workflow debe usar la ruta correcta.

## Ejemplo de build y transferencia

Este ejemplo solo construye y transfiere archivos; no completa por sí solo el despliegue en Pages.

```yaml
name: Build y deploy
on:
  push:
    branches: [main]
  workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: sitio
          path: dist/
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: sitio
          path: dist/
```

- `actions/checkout@v4`: descarga el repositorio.
- `npm install`: instala dependencias.
- `npm run build`: genera el sitio compilado.
- `actions/upload-artifact@v4`: guarda el contenido generado.
- `name: sitio`: asigna el nombre `sitio` al artefacto.
- `path: dist/`: indica la carpeta generada que se conserva.
- `needs: build`: hace que el job `deploy` espere a que termine el job `build`.
- `actions/download-artifact@v4`: descarga el artefacto en el siguiente job.
- `path: dist/`: indica dónde se coloca el artefacto descargado.

## Si Pages falla

1. Comprueba que `index.html` está en la carpeta publicada.
2. Revisa la rama y la carpeta en **Settings → Pages**.
3. Confirma que los cambios están subidos con `git status` y `git log --oneline`.
4. Revisa el workflow si se despliega con Actions.
5. Comprueba las rutas relativas de CSS, JavaScript e imágenes.

- **Commit:** guarda cambios en el historial.
- **Push:** sube commits al remoto.
- **Build:** genera archivos finales.
- **Artifact:** conserva archivos generados.
- **Deploy:** publica la aplicación.
