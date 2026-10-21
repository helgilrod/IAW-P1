# Ejemplos de workflows

Plantillas listas para adaptar: cambia nombres, ramas y versiones por los del enunciado.

## C con Make
~~~yaml
name: Compilar programa C
on:
  push:
    branches: [main]
  workflow_dispatch:
jobs:
  compilar:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Ver GCC
        run: gcc --version
      - name: Compilar
        run: make
      - name: Probar
        run: make test
      - uses: actions/upload-artifact@v4
        with:
          name: estadistiques
          path: estadistiques
          retention-days: 7
~~~

Makefile de ejemplo; los comandos de las reglas empiezan con una tabulación real:
~~~makefile
CC = gcc
CFLAGS = -Wall -Wextra -Werror

all: estadistiques

estadistiques: estadistiques.c
	$(CC) $(CFLAGS) estadistiques.c -o estadistiques

test: estadistiques
	./estadistiques

clean:
	rm -f estadistiques
~~~

## Python + PyInstaller
~~~yaml
name: Compilar Python
on:
  push:
    branches: [main]
  workflow_dispatch:
jobs:
  build-linux:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: pip install pyinstaller
      - run: pyinstaller --onefile comprova_desplegaments.py
~~~
Para compilar en varios sistemas define jobs con ubuntu-latest, macos-latest y windows-latest. El ejecutable de dist/ es específico del sistema donde se compila.

## Linter JavaScript (actividad original)
~~~yaml
name: main_workflow
on:
  push:
    branches: [nodejs_linter_job]
jobs:
  linter-execution:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: github/super-linter@v4
        env:
          DEFAULT_BRANCH: nodejs_example
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          LINTER_RULES_PATH: /
          JAVASCRIPT_ES_CONFIG_FILE: .eslintrc.json
          VALIDATE_JAVASCRIPT_ES: true
~~~

## Jest
~~~yaml
name: Jest tests
on:
  push:
    branches: [nodejs_jest_job]
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
package.json debe definir el script de test, por ejemplo "test": "jest".

## Variable de entorno
~~~yaml
jobs:
  environment_test_execution:
    runs-on: ubuntu-latest
    env:
      DESTINATION_PERSON: "2DAW"
    steps:
      - run: echo "Hello $DESTINATION_PERSON"
~~~

## Build y artefacto
~~~yaml
- run: npm install
- run: npm run build
- uses: actions/upload-artifact@v4
  with:
    name: build
    path: dist/
~~~
La carpeta de salida depende del proyecto; comprueba que coincide con la ruta real.

---
[← 06 Tests de Python en Actions](06-tests-python-actions.md) · [Índice](../README.md) · [08 Pages y despliegues →](08-pages-despliegues.md)
