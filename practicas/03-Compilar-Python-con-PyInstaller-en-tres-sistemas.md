# Práctica 3. Compilar un programa Python en Linux, macOS y Windows

## Objetivo
Utilizar GitHub Actions para ejecutar PyInstaller en tres sistemas operativos y generar un ejecutable para cada plataforma.

## 1. Comprobar el proyecto y crear la carpeta
Desde la carpeta raíz del repositorio:

```bash
pwd
ls -la
test -f comprova_desplegaments.py && echo "Script encontrado"
mkdir -p .github/workflows
```

- `pwd`: muestra la ruta actual.
- `ls -la`: lista los archivos del proyecto.
- `test -f ...`: comprueba que el script existe; si no aparece el mensaje, revisa la ruta y el nombre.
- `mkdir -p .github/workflows`: crea la carpeta para los workflows, incluidos los directorios intermedios que falten.

## 2. Crear el workflow desde la terminal
Ejecuta el bloque completo en la raíz del repositorio. Crea el archivo `.github/workflows/build-python.yml`:

```bash
cat > .github/workflows/build-python.yml <<'EOF'
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
      - uses: actions/upload-artifact@v4
        with:
          name: ejecutable-linux
          path: dist/comprova_desplegaments*

  build-macos:
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: pip install pyinstaller
      - run: pyinstaller --onefile comprova_desplegaments.py
      - uses: actions/upload-artifact@v4
        with:
          name: ejecutable-macos
          path: dist/comprova_desplegaments*

  build-windows:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: pip install pyinstaller
      - run: pyinstaller --onefile comprova_desplegaments.py
      - uses: actions/upload-artifact@v4
        with:
          name: ejecutable-windows
          path: dist/comprova_desplegaments*
EOF
cat .github/workflows/build-python.yml
```

- `cat > ... <<'EOF'`: escribe el YAML completo en el archivo.
- `cat ...`: muestra el contenido para comprobar que se guardó correctamente.
- Cada job usa un runner diferente; el artefacto permite descargar el ejecutable al terminar el job.

## 3. Guardar y subir el workflow

```bash
git status
git add .github/workflows/build-python.yml
git diff --staged
git commit -m "Añadir compilación Python multiplataforma"
git push
```

- `git status`: revisa los cambios detectados.
- `git add ...`: prepara el workflow.
- `git diff --staged`: permite revisar exactamente el archivo preparado.
- `git commit ...`: guarda el cambio en el historial.
- `git push`: sube el commit a la rama que tiene configurado el remoto de seguimiento.

Si aún no hay un remoto configurado, consulta `git remote -v` y sigue las instrucciones del repositorio proporcionado por el profesor; no inventes una URL.

## 4. Revisar la ejecución
En GitHub abre **Actions** y entra en la ejecución más reciente. Los tres jobs deben terminar correctamente. Descarga cada artefacto y comprueba que corresponde al sistema operativo adecuado.

## Ideas importantes y errores
- Un ejecutable creado en Linux no sustituye al de Windows o macOS: cada sistema necesita su propio build.
- Si falla, comprueba el nombre del script, la versión de Python, la instalación de dependencias y el log del job.
- La ruta de artefactos debe coincidir con el nombre que genera PyInstaller.
- Si el enunciado exige una rama específica, ajusta `on.push.branches` a esa rama.
