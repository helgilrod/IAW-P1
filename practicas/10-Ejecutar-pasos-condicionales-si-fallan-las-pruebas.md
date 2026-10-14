# Práctica 10. Ejecutar un paso cuando fallan las pruebas

## Objetivo
Controlar si un paso posterior se ejecuta según el resultado de los pasos anteriores.

## 1. Definir el paso de test
```yaml
- name: Ejecutar pruebas
  id: test
  run: npm test
```
El comando `npm test` devuelve un código de salida. Normalmente, cero significa éxito y un valor distinto de cero indica error. El identificador `test` permite referenciar el paso en otras expresiones.

## 2. Añadir un aviso cuando hay un fallo
```yaml
- name: Avisar de fallo
  if: failure()
  run: echo "Las pruebas han fallado"
```
La condición `if: failure()` hace que se ejecute el aviso si ha fallado un paso previo relevante. No corrige el error ni convierte las pruebas en correctas; solo permite reaccionar ante él.

## 3. Workflow completo de ejemplo
```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v3
        with:
          node-version: 20
      - run: npm install
      - name: Ejecutar pruebas
        id: test
        run: npm test
      - name: Avisar de fallo
        if: failure()
        run: echo "Las pruebas han fallado"
```

## 4. Diferencias entre condiciones
- `success()`: el paso se ejecuta si los pasos anteriores relevantes han ido bien.
- `failure()`: se ejecuta si ha ocurrido un fallo anterior.
- `always()`: intenta ejecutar el paso aunque haya fallos o cancelaciones previas. Úsala con criterio, sobre todo en tareas de despliegue.

Cuando un job depende de otro, `needs` define esa dependencia. Si un job debe evaluarse aunque falle una dependencia, la práctica puede requerir `if: always()`.

## 5. Probar
Primero ejecuta las pruebas correctamente. Después, en una rama de práctica, provoca un error controlado y comprueba que aparece el aviso. Revierte el cambio y confirma que la versión final vuelve a pasar.