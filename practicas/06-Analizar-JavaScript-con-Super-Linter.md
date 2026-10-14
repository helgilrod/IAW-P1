# Práctica 6. Analizar JavaScript con Super-Linter

## Objetivo
Añadir un workflow que ejecute Super-Linter al subir cambios a una rama concreta.

## 1. Comprobar el repositorio

```bash
pwd
ls -la
git status
git remote -v
git branch --show-current
```

- `pwd`: muestra la carpeta actual.
- `ls -la`: lista los archivos del proyecto.
- `git status`: comprueba el estado de la copia local.
- `git remote -v`: muestra el remoto configurado.
- `git branch --show-current`: muestra la rama actual.

## 2. Crear o cambiar a la rama

Si la rama no existe:

```bash
git switch -c nodejs_linter_job
```

Si ya existe localmente:

```bash
git switch nodejs_linter_job
```

Comprueba la rama:

```bash
git branch --show-current
git status
```

- `git switch -c nodejs_linter_job`: crea la rama y cambia a ella.
- `git switch nodejs_linter_job`: cambia a la rama existente.
- `git branch --show-current`: confirma el nombre de la rama actual.
- `git status`: muestra si hay cambios pendientes.

## 3. Comprobar la configuración de JavaScript

```bash
find . -maxdepth 2 -type f
test -f .eslintrc.json && echo "Configuración ESLint encontrada"
mkdir -p .github/workflows
```

- `find ...`: lista archivos para localizar el código JavaScript y la configuración.
- `test -f .eslintrc.json ...`: comprueba si existe el archivo de configuración esperado por este ejemplo.
- `mkdir -p .github/workflows`: crea la carpeta donde GitHub Actions busca los workflows.

Si el enunciado proporciona una configuración distinta, utiliza la ruta y el nombre exactos que indique el profesor.

## 4. Crear el workflow
Guarda el archivo como `.github/workflows/main_workflow.yml`:

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
      - uses: actions/checkout@v2
      - name: Linter
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

- `cat > ... <<'EOF'`: crea el archivo con la configuración YAML.
- `cat ...`: muestra el archivo para revisar su contenido.
- `on.push.branches`: limita la ejecución a la rama `nodejs_linter_job`.
- `runs-on`: selecciona la máquina donde se ejecuta el job.
- `uses`: ejecuta acciones reutilizables.
- `env`: define las variables de entorno para configurar el linter.
- `GITHUB_TOKEN`: utiliza el token automático de GitHub Actions; no pegues contraseñas ni tokens privados en el archivo.

## 5. Confirmar y subir

```bash
git status
git add .github/workflows/main_workflow.yml
git diff --staged
git commit -m "Añadir job de linter"
git push -u origin nodejs_linter_job
```

- `git status`: comprueba los cambios.
- `git add ...`: prepara el workflow.
- `git diff --staged`: revisa el contenido que se confirmará.
- `git commit -m ...`: crea el commit.
- `git push -u origin nodejs_linter_job`: sube la rama y configura el seguimiento remoto.

## 6. Revisar el resultado
En GitHub abre **Actions**, entra en la ejecución del workflow y examina el log. Comprueba que el workflow se ha ejecutado en la rama correcta y que el archivo de configuración se encuentra en la ruta esperada.

## Diagnóstico
- Si el workflow no se inicia, revisa el nombre de la rama dentro de `on.push.branches`.
- Si no encuentra las reglas, comprueba `LINTER_RULES_PATH` y `JAVASCRIPT_ES_CONFIG_FILE`.
- Si falla el linter, lee el log para localizar el archivo y la regla que produce el error.
- La versión v4 de Super-Linter y sus variables se mantienen como parte del material de esta práctica; si el profesor exige esas versiones, no las cambies por tu cuenta.
