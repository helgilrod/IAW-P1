# Práctica 3. Compilar un programa Python en Linux, macOS y Windows

## Objetivo
Utilizar GitHub Actions para ejecutar PyInstaller en tres sistemas operativos y generar un ejecutable para cada plataforma.

**Teoría:** [GitHub Actions y YAML](../teoria/05-actions-yaml.md) · [Tests de Python en Actions](../teoria/06-tests-python-actions.md) · [Ejemplos de workflows](../teoria/07-ejemplos-actions.md)

## 1. Preparar el repositorio
Comprueba que `comprova_desplegaments.py` está en la raíz. Crea la carpeta de workflows:
```bash
mkdir -p .github/workflows
```
GitHub Actions detecta automáticamente los archivos YAML de esa carpeta.

## 2. Crear el workflow
Guarda lo siguiente como `.github/workflows/build-python.yml`:
```yaml
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
```
Cada job se ejecuta en una máquina distinta. `checkout` descarga el código; `setup-python` instala la versión de Python solicitada; `pip install` instala PyInstaller; y `--onefile` solicita un ejecutable de archivo único.

## 3. Guardar y subir el workflow
```bash
git add .github/workflows/build-python.yml
git commit -m "Añadir compilación Python multiplataforma"
git push
```
Si el enunciado limita la ejecución a una rama concreta, especifica esa rama dentro de `on.push.branches`.

## 4. Revisar la ejecución
En GitHub abre **Actions** y entra en la ejecución más reciente. Los tres jobs deben aparecer y terminar correctamente. PyInstaller suele escribir el resultado en `dist/`, pero en este ejemplo el ejecutable queda en el runner temporal: para conservarlo después de acabar el job, hay que añadir un paso `actions/upload-artifact` con la ruta correcta.

## 5. Variantes habituales
**Una versión de Python distinta en cada job.** Cambia solo `python-version` en cada uno y ponla siempre entre comillas (`'3.10'`; sin comillas, YAML lo lee como el número 3.1).

**Ejecutar tests además de compilar, y conservar el ejecutable.** Así quedan los pasos de un job (el de Linux); en los otros dos cambia el `name` del artefacto para que no choquen:
```yaml
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: python -m unittest -v
      - run: pip install pyinstaller
      - run: pyinstaller --onefile comprova_desplegaments.py
      - uses: actions/upload-artifact@v4
        with:
          name: ejecutable-linux
          path: dist/
```
Si algún test falla, el job sale en rojo y no se compila.

**Matriz.** Sustituye los tres jobs por uno solo con `strategy.matrix`. Úsala solo si el enunciado no exige tres jobs con nombres concretos.

Más detalle en [Tests de Python en GitHub Actions](../teoria/06-tests-python-actions.md).

## Ideas importantes y errores
Un ejecutable creado en Linux no sustituye al de Windows o macOS: cada sistema necesita su propio build. Si falla, comprueba el nombre del script, la versión de Python, la instalación de dependencias y el log del job. Añade pruebas del ejecutable solo con comandos adecuados para cada sistema operativo.

---
[← Práctica 2](02-Publicar-web-personal-con-GitHub-Pages.md) · [Índice de prácticas](README.md) · [Práctica 4 →](04-Compilar-C-con-Makefile-y-guardar-artefacto.md)
