# Práctica 7. Pruebas con Jest

## Paso 1. Crear rama
~~~bash
git switch -c nodejs_jest_job
~~~

## Paso 2. Crear sum.js
~~~javascript
function sum(a, b) {
  return a + b;
}
module.exports = sum;
~~~

## Paso 3. Crear sum.test.js
~~~javascript
const sum = require('./sum');
test('suma 1 + 2 es igual a 3', () => {
  expect(sum(1, 2)).toBe(3);
});
~~~

## Paso 4. Configurar package.json
~~~json
{
  "name": "nodejs-jest-practice",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "test": "jest"
  }
}
~~~
Si ya existe package.json, añade el script sin borrar el resto de propiedades.

## Paso 5. Workflow .github/workflows/jest.yml
~~~yaml
name: Jest tests
on:
  push:
    branches: [nodejs_jest_job]
  workflow_dispatch:
jobs:
  test-execution:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v3
        with:
          node-version: 20
      - run: npm install jest
      - run: npm test
~~~

## Paso 6. Commit y verificación
~~~bash
git add sum.js sum.test.js package.json .github/workflows/jest.yml
git commit -m "Añadir pruebas Jest"
git push -u origin nodejs_jest_job
~~~
En Actions comprueba que instalación y npm test terminan correctamente.