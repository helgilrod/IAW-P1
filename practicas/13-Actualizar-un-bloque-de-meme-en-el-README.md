# Práctica 13. Crear una acción que actualiza un meme en el README

## Objetivo
Seleccionar un texto según el resultado de los tests y actualizar solo una parte del README, sin borrar el resto.

## 1. Añadir marcadores al README
```markdown
<!-- MEME-START -->
Contenido inicial del meme
<!-- MEME-END -->
```
Los comentarios HTML marcan el inicio y el final de la zona que la acción puede modificar. No elimines los marcadores al actualizar el contenido.

## 2. Declarar inputs en action.yml
```yaml
name: Actualizar meme del README
description: Cambia el meme según el resultado de pruebas
inputs:
  frase_positiva:
    description: Texto si las pruebas pasan
    required: true
  frase_negativa:
    description: Texto si las pruebas fallan
    required: true
  resultat_tests:
    description: Resultado de pruebas
    required: true
runs:
  using: node20
  main: index.js
```

## 3. Implementar index.js
La acción debe:
1. Leer los inputs con `@actions/core`.
2. Elegir la frase positiva o negativa según el resultado de los tests.
3. Construir la URL del meme usando el servicio y formato indicados en el material de clase.
4. Leer `README.md` con el módulo `fs`.
5. Sustituir exclusivamente el texto situado entre los marcadores.
6. Guardar el archivo y marcar errores con `core.setFailed`.

Antes de escribir, comprueba que ambos marcadores existen. La URL exacta del servicio depende del enunciado: no la inventes ni uses un endpoint distinto.

## 4. Crear el workflow
Ejemplo de estructura para `.github/workflows/modify_readme.yml`:
```yaml
name: modify_readme
on:
  push:
    branches: [github_action_readme]
  workflow_dispatch:
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Ejecutar pruebas
        id: tests
        run: npm test

  update_readme:
    needs: test
    if: always()
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Actualizar meme
        uses: ./readme-meme-action
        with:
          frase_positiva: "Tot correcte!"
          frase_negativa: "Cal revisar els tests"
          resultat_tests: ${{ needs.test.result }}
```
`needs: test` establece la dependencia; `if: always()` permite que el job de actualización se evalúe incluso si las pruebas fallan. Cada job tiene su propio runner, por eso el segundo vuelve a descargar el repositorio.

## 5. Guardar el cambio en GitHub
Si la práctica exige que el README actualizado quede en el repositorio, añade el permiso mínimo necesario (normalmente `contents: write`) y los pasos de identidad Git, commit y push. Sin ellos, el archivo modificado solo existe en el runner y se pierde al terminar.

## 6. Verificar
Prueba con tests correctos y con un fallo controlado. Comprueba que cambia el meme y que el resto del README permanece igual. Revisa los outputs reales de los jobs y los parámetros exactos de la acción que pide el profesor.