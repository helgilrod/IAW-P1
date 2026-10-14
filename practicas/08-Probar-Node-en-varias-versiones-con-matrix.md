# Práctica 8. Probar un proyecto con varias versiones de Node.js

## Objetivo
Una matriz evita duplicar un workflow: GitHub repite el mismo job para cada versión declarada.

## 1. Crear la rama
```bash
git switch -c nodejs_nodeversions_job
```

## 2. Crear `.github/workflows/node-versions.yml`
```yaml
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
```

## 3. Qué hace cada parte
- `on` determina cuándo arranca: al subir cambios a la rama indicada o manualmente.
- `runs-on` selecciona Ubuntu como máquina virtual.
- `strategy.matrix` crea una ejecución por cada versión de Node.js.
- `matrix.node-version` inserta en cada ejecución el valor correspondiente.
- `npm install` instala las dependencias del `package.json`.
- `npm test` ejecuta el script `test`, que debe existir en `package.json`.

## 4. Guardar los cambios
```bash
git add .github/workflows/node-versions.yml
git commit -m "Probar varias versiones de Node"
git push -u origin nodejs_nodeversions_job
```

## 5. Verificar y solucionar errores
En **Actions** deberían aparecer ejecuciones separadas por versión. Si falla una, abre su log: puede ser una dependencia incompatible con esa versión o que falte el script `test`. Las versiones 16, 18 y 20 son las del ejemplo; utiliza las exactas que pida el profesor. En proyectos nuevos, comprueba qué versiones siguen mantenidas.