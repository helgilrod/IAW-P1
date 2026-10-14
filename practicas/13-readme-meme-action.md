# Práctica 13. Acción que actualiza un meme en README

## Paso 1. Reservar bloque en README
~~~markdown
<!-- MEME-START -->
Contenido inicial del meme
<!-- MEME-END -->
~~~
La acción debe sustituir solo el contenido entre marcadores.

## Paso 2. Definir action.yml
~~~yaml
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
~~~

## Paso 3. Implementar index.js
La acción debe:
1. Leer los tres inputs mediante @actions/core.
2. Elegir frase positiva o negativa según el resultado.
3. Construir la URL de meme con el servicio de la práctica.
4. Leer README.md.
5. Reemplazar únicamente el contenido entre MEME-START y MEME-END.
6. Escribir el archivo y reportar éxito o fallo.

Usa fs para leer/escribir. El servicio y la plantilla exacta de URL deben coincidir con el material de clase.

## Paso 4. Workflow
~~~yaml
name: modify_readme
on:
  push:
    branches: [github_action_readme]
  workflow_dispatch:
jobs:
  test:
    runs-on: ubuntu-latest
    outputs:
      result: ${{ steps.tests.outcome }}
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
          resultat_tests: ${{ needs.test.outputs.result }}
~~~

## Paso 5. Permitir commit del README
Si el workflow debe subir el README modificado, configura solo los permisos necesarios (contents: write), la identidad Git y un paso de commit/push. El segundo job necesita su propio checkout porque cada job tiene un runner separado.

## Paso 6. Verificar
Prueba con tests correctos y con un fallo controlado. Comprueba que el bloque cambia sin borrar el resto del README. Si los outputs no están disponibles cuando falla el job, utiliza el contexto de resultado de job que indique la actividad.