# Práctica 3. Compilar un programa Python para Linux, macOS y Windows con PyInstaller

## Objetivo
Crear un workflow de GitHub Actions que ejecute el script `comprova_desplegaments.py` en tres sistemas operativos, genere un ejecutable con PyInstaller y publique cada ejecutable como artefacto descargable.

## 1. Abrir el repositorio y comprobar los archivos

Abre la terminal en la raíz del repositorio:

```bash
pwd
ls -la
git status
test -f comprova_desplegaments.py && echo "Script encontrado"
mkdir -p .github/workflows
```

- `pwd`: muestra la ruta actual.
- `ls -la`: lista archivos visibles y ocultos.
- `git status`: comprueba rama y cambios pendientes.
- `test -f ...`: verifica que el script existe. Si no aparece el mensaje, localiza el nombre correcto con `find . -maxdepth 3 -type f`.
- `mkdir -p .github/workflows`: crea la carpeta de workflows.

No inventes ni cambies el nombre del script si el enunciado exige uno concreto.

## 2. Crear el workflow

Crea `.github/workflows/build-python.yml` desde la raíz del repositorio:

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
      - name: Descargar repositorio
        uses: actions/checkout@v4
      - name: Preparar Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - name: Instalar PyInstaller
        run: python -m pip install --upgrade pip pyinstaller
      - name: Crear ejecutable
        run: pyinstaller --onefile comprova_desplegaments.py
      - name: Guardar artefacto Linux
        uses: actions/upload-artifact@v4
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
      - run: python -m pip install --upgrade pip pyinstaller
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
      - run: python -m pip install --upgrade pip pyinstaller
      - run: pyinstaller --onefile comprova_desplegaments.py
      - uses: actions/upload-artifact@v4
        with:
          name: ejecutable-windows
          path: dist/comprova_desplegaments*
EOF
cat .github/workflows/build-python.yml
```

- `cat > archivo <<'EOF'`: escribe el contenido entre `EOF` y `EOF` en el archivo.
- `name`: nombre visible del workflow.
- `on.push`: inicia el workflow al subir cambios.
- `workflow_dispatch`: permite iniciarlo manualmente desde Actions.
- `jobs`: define trabajos independientes.
- `runs-on`: elige la máquina Linux, macOS o Windows.
- `actions/checkout`: descarga el código en la máquina de ejecución.
- `actions/setup-python`: instala la versión indicada de Python.
- `pyinstaller --onefile`: genera un ejecutable de un solo archivo.
- `actions/upload-artifact`: conserva el ejecutable para descargarlo desde la ejecución.
- `cat ...`: comprueba el contenido que acabas de guardar.

## 3. Comprobar el YAML antes de subirlo

```bash
git status
cat .github/workflows/build-python.yml
```

Revisa especialmente:
- la indentación con espacios;
- que todos los pasos estén dentro de `steps`;
- que el nombre del script sea exactamente el del repositorio;
- que cada job suba un artefacto con un nombre diferente.

## 4. Guardar y subir el workflow

```bash
git add .github/workflows/build-python.yml
git diff --staged
git commit -m "Añadir compilacion Python multiplataforma"
git push
```

- `git add`: prepara el workflow.
- `git diff --staged`: permite revisar el archivo antes del commit.
- `git commit`: guarda la versión.
- `git push`: publica el commit en la rama que sigue el remoto configurado.

Si todavía no hay remoto, ejecuta `git remote -v` y usa la URL real del repositorio facilitado por el profesor. No inventes una URL. Si el enunciado exige una rama concreta, cámbiate a esa rama antes de editar y subir.

## 5. Ejecutar y revisar Actions

1. Abre el repositorio en GitHub.
2. Entra en **Actions**.
3. Selecciona **Compilar script Python**.
4. Abre la ejecución más reciente.
5. Comprueba los tres jobs: Linux, macOS y Windows.
6. Abre los logs si algún paso falla.
7. Cuando termine correctamente, descarga cada artefacto desde la página de la ejecución.

## 6. Diagnóstico

- **No encuentra el script:** revisa el nombre y la ubicación del archivo.
- **Falla la instalación:** consulta el log del paso de pip.
- **No se genera el artefacto:** confirma que el archivo aparece en `dist/` y que el patrón de `path` coincide.
- **Un ejecutable no funciona en otro sistema:** es normal; los ejecutables se generan para su sistema operativo.
- **No se ejecuta el workflow:** comprueba que el YAML está en `.github/workflows/`, que se ha hecho push y que la rama coincide con el filtro configurado.
