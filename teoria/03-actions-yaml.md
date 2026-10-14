# GitHub Actions y YAML

## Conceptos
- Workflow: archivo de automatización.
- Evento: activa el workflow.
- Job: conjunto de pasos.
- Runner: máquina donde se ejecuta un job.
- Step: paso dentro de un job.
- Action: componente reutilizable, normalmente llamado con uses.
- Artifact: archivo generado y conservado por el workflow.

Los workflows se guardan en .github/workflows/nombre.yml o .yaml.

## Plantilla básica
~~~yaml
name: Mi workflow

on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Descargar repositorio
        uses: actions/checkout@v4
      - name: Mostrar mensaje
        run: echo "Workflow ejecutado"
~~~

## Elementos
- name: nombre del workflow o paso.
- on: eventos que lo activan.
- push: activación al subir cambios.
- branches: limita a las ramas indicadas.
- workflow_dispatch: ejecución manual.
- jobs: conjunto de trabajos.
- runs-on: runner (ubuntu-latest, windows-latest o macos-latest).
- steps: pasos en orden dentro del job.
- uses: ejecutar una acción.
- run: ejecutar comando de terminal.
- with: parámetros de una acción.
- env: variables de entorno.
- needs: dependencia entre jobs.
- if: condición.
- permissions: permisos del token.
- strategy.matrix: repetir job con varios valores.
- secrets.NOMBRE: leer un secreto configurado en GitHub, usando la expresión de Actions correspondiente.

## Eventos
~~~yaml
on:
  push:
    branches: [main]
  workflow_dispatch:
~~~
La rama debe coincidir exactamente con el enunciado.

## uses, run y with
~~~yaml
- uses: actions/setup-python@v5
  with:
    python-version: '3.12'
- run: pip install pyinstaller
~~~
La primera parte usa una acción y le pasa una versión de Python; la segunda ejecuta un comando.

## Variables y secretos
~~~yaml
env:
  DESTINATION_PERSON: "2DAW"
steps:
  - run: echo "$DESTINATION_PERSON"
  - name: Ejecutar con secreto
    env:
      TOKEN: ${{ secrets.TELEGRAM_TOKEN }}
    run: python enviar_mensaje.py
~~~
Configura los secretos desde GitHub; nunca escribas el valor del token en YAML.

## Dependencias
~~~yaml
jobs:
  linter:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "Lint"
  tests:
    needs: linter
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: echo "Tests"
~~~
Los jobs sin dependencia pueden ejecutarse en paralelo. needs hace que tests espere al job linter.

## Condiciones
- if: always(): ejecutar aunque haya fallos previos.
- if: success(): ejecutar si los pasos anteriores han tenido éxito.
- if: failure(): ejecutar si falla un paso previo relevante.
- if: steps.test.outcome == 'failure': comprobar si falló el paso con id test.

~~~yaml
steps:
  - name: Ejecutar pruebas
    id: test
    run: npm test
  - name: Avisar del error
    if: failure()
    run: echo "Las pruebas han fallado"
~~~

## Matriz
~~~yaml
strategy:
  matrix:
    node-version: [16, 18, 20]
steps:
  - uses: actions/setup-node@v3
    with:
      node-version: ${{ matrix.node-version }}
~~~

## Artefactos
~~~yaml
- uses: actions/upload-artifact@v4
  with:
    name: programa
    path: estadistiques
    retention-days: 7
~~~
~~~yaml
- uses: actions/download-artifact@v4
  with:
    name: programa
    path: ./descarga
~~~
El archivo debe existir en la ruta indicada. Un artefacto no es un commit ni una publicación en Pages.

## Errores frecuentes
Indentación incorrecta o tabulaciones; workflow fuera de .github/workflows; rama equivocada; confundir uses y run; olvidar checkout; dependencias no instaladas; ruta de artefacto inexistente; secreto no configurado; olvidar needs. En examen, respeta las versiones indicadas por el profesor.