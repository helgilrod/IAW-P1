# GitHub Pages y despliegues

GitHub Pages publica sitios estáticos con HTML, CSS y JavaScript del lado del cliente.

## Publicación desde una rama
Crea index.html, guárdalo y sube los cambios:
~~~bash
git add .
git commit -m "Crear página inicial"
git push
~~~
En GitHub: Settings → Pages, selecciona la rama (normalmente main) y la carpeta /(root) o /docs; guarda y espera al despliegue.

## Nombres y URL
- Repositorio personal: usuario.github.io → https://usuario.github.io/
- Repositorio de proyecto: mi-proyecto → https://usuario.github.io/mi-proyecto/

## Publicar desde docs
~~~text
repositorio/
├── README.md
└── docs/
    ├── index.html
    ├── css/estilos.css
    └── js/script.js
~~~
En Settings → Pages selecciona main y /docs. La carpeta elegida debe contener index.html.

## Compilar y desplegar no son lo mismo
- Checkout: descarga código.
- Instalar: prepara dependencias (npm install).
- Build: genera archivos finales (npm run build).
- Test: ejecuta pruebas (npm test).
- Artifact: conserva archivos generados.
- Deploy: publica la aplicación.

En React, la salida suele ser build/ o dist/, según la herramienta. El workflow debe usar la ruta correcta.

## Ejemplo de build y transferencia
Este ejemplo solo construye y transfiere archivos; no completa por sí solo el despliegue en Pages.
~~~yaml
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
~~~

## Si Pages falla
1. Comprueba que index.html está en la carpeta publicada.
2. Revisa rama y carpeta en Settings → Pages.
3. Confirma que los cambios están subidos.
4. Revisa el workflow si se despliega con Actions.
5. Comprueba rutas relativas de CSS, JavaScript e imágenes.

Commit guarda cambios en historial; push sube commits; build genera archivos; artifact conserva archivos generados; deploy publica la aplicación.