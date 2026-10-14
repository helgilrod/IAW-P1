# Ejemplos de workflows

## C con Make

```yaml
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
```

- `name`: asigna el nombre visible al workflow.
- `on.push.branches`: ejecuta el workflow cuando se suben cambios a `main`.
- `workflow_dispatch`: permite iniciar la ejecución manualmente desde GitHub.
- `jobs`: contiene los trabajos del workflow.
- `runs-on: ubuntu-latest`: ejecuta el trabajo en un runner Ubuntu.
- `actions/checkout@v4`: descarga el contenido del repositorio en el runner.
- `gcc --version`: muestra la versión del compilador GCC.
- `make`: ejecuta el objetivo predeterminado del Makefile para compilar.
- `make test`: ejecuta el objetivo de prueba definido en el Makefile.
- `actions/upload-artifact@v4`: guarda archivos generados para poder descargarlos al terminar el job.
- `name: estadistiques`: da nombre al artefacto.
- `path: estadistiques`: indica qué archivo se guarda.
- `retention-days: 7`: conserva el artefacto durante siete días.

Makefile de ejemplo; los comandos de las reglas empiezan con una tabulación real:

```makefile
CC = gcc
CFLAGS = -Wall -Wextra -Werror

all: estadistiques

estadistiques: estadistiques.c
	$(CC) $(CFLAGS) estadistiques.c -o estadistiques

test: estadistiques
	./estadistiques

clean:
	rm -f estadistiques
```

- `CC = gcc`: define el compilador.
- `CFLAGS = -Wall -Wextra -Werror`: activa advertencias y hace que se traten como errores.
- `all: estadistiques`: establece `estadistiques` como objetivo predeterminado.
- `estadistiques: estadistiques.c`: indica que el ejecutable depende del archivo fuente.
- `$(CC) ... -o estadistiques`: compila el archivo C y genera el ejecutable.
- `test: estadistiques`: indica que para probar el programa primero debe existir el ejecutable.
- `./estadistiques`: ejecuta el programa compilado.
- `clean`: define el objetivo de limpieza.
- `rm -f estadistiques`: elimina el ejecutable si existe.

## Python + PyInstaller

```yaml
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
```

- `actions/checkout@v4`: descarga el código fuente.
- `actions/setup-python@v5`: instala y configura Python en el runner.
- `python-version: '3.12'`: selecciona la versión de Python.
- `pip install pyinstaller`: instala PyInstaller.
- `pyinstaller --onefile comprova_desplegaments.py`: crea un ejecutable de archivo único a partir del script.
- Para compilar en varios sistemas, define jobs separados con `ubuntu-latest`, `macos-latest` y `windows-latest`.
- El ejecutable de `dist/` es específico del sistema donde se compila.

## Linter JavaScript (actividad original)

```yaml
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
```

- `name: main_workflow`: asigna el nombre del workflow.
- `on.push.branches`: limita la ejecución a la rama `nodejs_linter_job`.
- `actions/checkout@v2`: descarga el repositorio en el runner.
- `github/super-linter@v4`: ejecuta Super-Linter en la versión indicada en el material.
- `DEFAULT_BRANCH`: indica la rama principal de referencia para el linter.
- `GITHUB_TOKEN`: utiliza el token automático de GitHub Actions.
- `LINTER_RULES_PATH`: indica la ruta de las reglas de configuración.
- `JAVASCRIPT_ES_CONFIG_FILE`: indica el archivo de configuración de JavaScript.
- `VALIDATE_JAVASCRIPT_ES: true`: activa la validación de JavaScript con las reglas correspondientes.

## Jest

```yaml
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
```

- `actions/checkout@v4`: descarga el código.
- `actions/setup-node@v3`: configura Node.js.
- `node-version: 20`: selecciona Node.js 20.
- `npm install jest`: instala Jest.
- `npm test`: ejecuta el script de pruebas definido en `package.json`.
- `package.json` debe definir el script, por ejemplo `"test": "jest"`.

## Variable de entorno

```yaml
jobs:
  environment_test_execution:
    runs-on: ubuntu-latest
    env:
      DESTINATION_PERSON: "2DAW"
    steps:
      - run: echo "Hello $DESTINATION_PERSON"
```

- `env`: define variables de entorno disponibles en el job o paso correspondiente.
- `DESTINATION_PERSON`: guarda el texto `2DAW` en una variable.
- `echo "Hello $DESTINATION_PERSON"`: muestra un saludo que incluye el valor de la variable.

## Build y artefacto

```yaml
- run: npm install
- run: npm run build
- uses: actions/upload-artifact@v4
  with:
    name: build
    path: dist/
```

- `npm install`: instala las dependencias del proyecto.
- `npm run build`: ejecuta el script de compilación definido en `package.json`.
- `actions/upload-artifact@v4`: conserva los archivos generados.
- `name: build`: asigna el nombre `build` al artefacto.
- `path: dist/`: indica la carpeta que se guardará.
- La carpeta de salida depende del proyecto; comprueba que coincide con la ruta real.
