# Práctica 11. Construir y desplegar una aplicación React en Surge

## Objetivo
Separar la compilación de React y el despliegue en dos jobs. El segundo recibe el artefacto del primero.

## 1. Preparar el proyecto
```bash
git switch -c react_app_deploy
```
Comprueba que existe `package.json` y revisa qué carpeta genera el script `build`: según el proyecto puede ser `build/` o `dist/`.

## 2. Crear `.github/workflows/example_workflow.yml`
```yaml
name: example_workflow
on:
  push:
    branches: [react_app_deploy]
  workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Instalar dependencias
        run: npm install
      - name: Construir React
        run: npm run build
      - name: Guardar build
        uses: actions/upload-artifact@v3
        with:
          name: react-build
          path: build/

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Descargar build
        uses: actions/download-artifact@v4
        with:
          name: react-build
          path: build/
      - name: Desplegar en Surge
        uses: dswistowski/surge-sh-action@v1.1.0
        with:
          domain: TU_DOMINIO.surge.sh
          project: build/
          login: ${{ secrets.SURGE_LOGIN }}
          token: ${{ secrets.SURGE_TOKEN }}
```

## 3. Comprender el orden
El job `build` descarga el código, instala dependencias y genera archivos estáticos. `upload-artifact` conserva la carpeta entre jobs. Cada job se ejecuta en una máquina distinta, por lo que el segundo no puede usar automáticamente los archivos del primero. `needs: build` indica que el despliegue depende de una compilación terminada.

## 4. Configurar secretos y subir
En **Settings → Secrets and variables → Actions** crea `SURGE_LOGIN` y `SURGE_TOKEN`. Sustituye `TU_DOMINIO` por el subdominio correcto. No escribas credenciales en YAML.
```bash
git add .github/workflows/example_workflow.yml
git commit -m "Añadir build y deploy de React"
git push -u origin react_app_deploy
```

## 5. Comprobar
En Actions revisa los dos jobs. Si falla el build, el despliegue no debe ejecutarse. Si falla Surge, comprueba secretos, dominio, carpeta publicada y los inputs que admite la versión de la acción exigida por el profesor. Este ejemplo conserva las versiones de la práctica; no las cambies sin comprobar compatibilidad.