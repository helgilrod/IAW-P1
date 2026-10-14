# Práctica 7. Ejecutar pruebas automáticas con Jest

## Objetivo
Crear una función JavaScript pequeña, escribir una prueba para comprobar su resultado y hacer que GitHub Actions la ejecute automáticamente.

## 1. Crear una rama
```bash
git switch -c nodejs_jest_job
```
Trabajar en una rama separada permite practicar sin mezclar estos cambios con otros ejercicios.

## 2. Escribir la función
Crea `sum.js`:
```javascript
function sum(a, b) {
  return a + b;
}
module.exports = sum;
```
La función recibe dos valores y devuelve su suma. `module.exports` permite importarla desde otro archivo con CommonJS.

## 3. Escribir la prueba
Crea `sum.test.js`:
```javascript
const sum = require('./sum');

test('suma 1 + 2 es igual a 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```
`test` define una prueba; `expect` expresa el valor esperado y `toBe(3)` comprueba que el resultado es exactamente 3.

## 4. Configurar package.json
Si no existe, crea:
```json
{
  "name": "nodejs-jest-practice",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "test": "jest"
  }
}
```
Si ya existe, no lo sustituyas entero: añade `"test": "jest"` dentro de `scripts` y conserva las demás propiedades.

## 5. Crear el workflow
Guarda como `.github/workflows/jest.yml`:
```yaml
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
```
El workflow instala Node.js, instala Jest y ejecuta el script `test` de `package.json`. `workflow_dispatch` permite lanzarlo manualmente desde Actions.

## 6. Subir y comprobar
```bash
git add sum.js sum.test.js package.json .github/workflows/jest.yml
git commit -m "Añadir pruebas Jest"
git push -u origin nodejs_jest_job
```
En Actions, la instalación y las pruebas deben acabar correctamente. Para comprobar que la prueba realmente detecta errores, cambia temporalmente el resultado esperado, ejecuta el workflow y luego restaura el valor correcto.