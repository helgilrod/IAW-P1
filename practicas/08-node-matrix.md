# Práctica 8. Matriz de versiones Node.js

## Paso 1. Rama
~~~bash
git switch -c nodejs_nodeversions_job
~~~

## Paso 2. Workflow
Archivo .github/workflows/node-versions.yml:
~~~yaml
name: Node versions
on:
  push:
    branches: [nodejs_nodeversions_job]
  workflow_dispatch:
jobs:
  tests:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [16, 18, 20]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm install
      - run: npm test
~~~

## Paso 3. Commit y push
~~~bash
git add .github/workflows/node-versions.yml
git commit -m "Probar varias versiones de Node"
git push -u origin nodejs_nodeversions_job
~~~

## Paso 4. Verificar
En Actions, el job se repite para cada valor de la matriz. Si falla npm install o npm test, comprueba que existe package.json y que el script test está definido. Si el profesor dio versiones específicas de acciones, respétalas.