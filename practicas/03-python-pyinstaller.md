# Práctica 3. Python y PyInstaller multiplataforma

## Objetivo
Compilar comprova_desplegaments.py en Linux, macOS y Windows con GitHub Actions.

## Paso 1. Comprobar archivos
El repositorio debe contener comprova_desplegaments.py. Crea la carpeta .github/workflows.

## Paso 2. Crear .github/workflows/build-python.yml
~~~yaml
name: Compilar script Python
on:
  push:
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

  build-macos:
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: pip install pyinstaller
      - run: pyinstaller --onefile comprova_desplegaments.py

  build-windows:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: pip install pyinstaller
      - run: pyinstaller --onefile comprova_desplegaments.py
~~~
Si el enunciado restringe el evento a una rama, añade branches bajo push.

## Paso 3. Commit y push
~~~bash
git add .github/workflows/build-python.yml
git commit -m "Añadir compilación Python"
git push
~~~

## Paso 4. Revisar
En Actions revisa los tres jobs. El ejecutable suele generarse en dist/ y depende del sistema operativo del runner; un ejecutable Linux no equivale al de Windows o macOS.

## Paso 5. Pruebas
Si el enunciado pide probar el ejecutable, añade el paso de test con la ruta y el nombre correctos para cada sistema.