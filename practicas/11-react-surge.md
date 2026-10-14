# Práctica 11. Build de React y despliegue en Surge

## Paso 1. Crear rama
~~~bash
git switch -c react_app_deploy
~~~
Asegúrate de que el repositorio contiene el proyecto React y package.json.

## Paso 2. Crear .github/workflows/example_workflow.yml
~~~yaml
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
~~~

## Paso 3. Configurar secretos
En Settings → Secrets and variables → Actions añade SURGE_LOGIN y SURGE_TOKEN con los valores privados correspondientes. Nunca escribas los valores en el YAML.

## Paso 4. Commit y push
~~~bash
git add .github/workflows/example_workflow.yml
git commit -m "Añadir build y deploy de React"
git push -u origin react_app_deploy
~~~

## Paso 5. Verificar
El job deploy depende de build. Comprueba que npm run build genera la carpeta configurada: algunos proyectos producen build/ y otros dist/. Confirma los inputs exactos de la versión de la acción de Surge que indique el profesor; las acciones pueden variar en sus parámetros.