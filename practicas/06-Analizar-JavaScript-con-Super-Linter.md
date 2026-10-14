# Práctica 6. Analizar JavaScript con Super-Linter en GitHub Actions

## Objetivo
Crear una rama de trabajo, añadir un workflow de GitHub Actions que ejecute Super-Linter cuando se suban cambios a esa rama y comprobar el resultado desde la pestaña Actions.

> Mantén los nombres de rama, archivo de configuración y versiones que exija el enunciado. El ejemplo usa los nombres del material de esta práctica.

## 1. Comprobar el repositorio

```bash
pwd
ls -la
git status
git remote -v
git branch --show-current
```

- `pwd`: muestra la carpeta actual.
- `ls -la`: lista los archivos.
- `git status`: muestra los cambios pendientes.
- `git remote -v`: comprueba el remoto configurado.
- `git branch --show-current`: muestra la rama actual.

## 2. Crear o cambiar a la rama requerida

Si la rama todavía no existe:

```bash
git switch -c nodejs_linter_job
```

Si ya existe:

```bash
git switch nodejs_linter_job
```

Después verifica:

```bash
git branch --show-current
git status
```

- `git switch -c`: crea la rama y cambia a ella.
- `git switch`: cambia a una rama existente.
- No crees otra rama si ya existe una con el nombre exigido.

## 3. Localizar los archivos JavaScript y su configuración

```bash
find . -maxdepth 3 -type f
test -f .eslintrc.json && echo "Configuración ESLint encontrada"
mkdir -p .github/workflows
```

- `find`: lista archivos para localizar el JavaScript y los archivos de configuración.
- `test -f .eslintrc.json`: comprueba si existe la configuración indicada en el ejemplo.
- `mkdir -p .github/workflows`: crea la carpeta en la que GitHub Actions busca los workflows.

Si el enunciado proporciona otra ruta o archivo de configuración, usa ese nombre exacto.

## 4. Crear el workflow

Crea `.github/workflows/main_workflow.yml`:

```bash
cat > .github/workflows/main_workflow.yml <<'EOF'
name: main_workflow
on:
  push:
    branches: [nodejs_linter_job]
jobs:
  linter-execution:
    runs-on: ubuntu-latest
    steps:
      - name: Descargar repositorio
        uses: actions/checkout@v4
      - name: Ejecutar Super-Linter
        uses: github/super-linter@v4
        env:
          DEFAULT_BRANCH: nodejs_example
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          LINTER_RULES_PATH: /
          JAVASCRIPT_ES_CONFIG_FILE: .eslintrc.json
          VALIDATE_JAVASCRIPT_ES: true
EOF
cat .github/workflows/main_workflow.yml
```

- `name`: nombre visible del workflow.
- `on.push.branches`: hace que se ejecute al subir cambios a `nodejs_linter_job).
- `jobs`: agrupa los trabajos que debe realizar Actions.
- `runs-on`: selecciona la máquina de ejecución.
- `steps`: contiene las acciones que se ejecutan en orden.
- `actions/checkout`: descarga el repositorio en la máquina.
- `github/super-linter`: ejecuta el analizador de código.
- `GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}`: usa el token automático de Actions; no escribas contraseñas ni tokens privados en el YAML.
- `LINTER_RULES_PATH`: indica dónde están las reglas de configuración.
- `JAVASCRIPT_ES_CONFIG_FILE`: indica el archivo de configuración de JavaScript.
- `VALIDATE_JAVASCRIPT_ES: true`: activa la validación de JavaScript.
- `cat ...`: permite revisar el YAML antes de subirlo.

## 5. Revisar y subir el workflow

```bash
git status
git add .github/workflows/main_workflow.yml
git status
git diff --staged
git commit -m "Añadir job de linter"
git push -u origin nodejs_linter_job
```

- `git status`: comprueba el estado del repositorio.
- `git add`: prepara el archivo del workflow.
- `git diff --staged`: muestra los cambios preparados.
- `git commit`: guarda el workflow en un commit.
- `git push -u origin nodejs_linter_job`: sube la rama y configura el seguimiento remoto.

Si Git informa de que no existe `origin`, comprueba `git remote -v` y configura la URL real del repositorio siguiendo el enunciado.

## 6. Ejecutar y revisar el workflow

1. Abre el repositorio en GitHub.
2. Entra en **Actions**.
3. Selecciona `main_workflow).
4. Abre la ejecución más reciente.
5. Entra en el job `linter-execution`.
6. Comprueba si el paso de Super-Linter terminó correctamente.
7. Si falla, abre el log para ver qué archivo o regla provoca el error.

## 7. Diagnóstico

- **El workflow no se ejecuta:** revisa que el archivo esté en `.github/workflows/`, que se haya hecho push y que el nombre de la rama coincida.
- **No encuentra la configuración:** comprueba la ruta de `.eslintrc.json` y `LINTER_RULES_PATH`.
- **Falla la validación:** lee el log; corrige el código o la configuración indicada y vuelve a hacer commit y push.
- **No aparece el job:** valida la indentación YAML y revisa que el nombre del archivo termine en `.yml` o `.yaml`.
- **La práctica exige otra versión:** conserva la versión exacta del enunciado en lugar de cambiarla por esta de ejemplo.
