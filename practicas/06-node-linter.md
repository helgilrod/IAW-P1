# Práctica 6. Linter JavaScript con Super-Linter

## Paso 1. Rama
~~~bash
git switch -c nodejs_linter_job
~~~
Si ya existe, usa git switch nodejs_linter_job.

## Paso 2. Workflow
Archivo .github/workflows/main_workflow.yml:
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
      - name: Linter
        uses: github/super-linter@v4
        env:
          DEFAULT_BRANCH: nodejs_example
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          LINTER_RULES_PATH: /
          JAVASCRIPT_ES_CONFIG_FILE: .eslintrc.json
          VALIDATE_JAVASCRIPT_ES: true
~~~
Conserva las versiones originales que indica la práctica.

## Paso 3. Revisar configuración
Comprueba que existe .eslintrc.json donde espera el linter y que DEFAULT_BRANCH coincide con la rama base real.

## Paso 4. Commit y push
~~~bash
git add .github/workflows/main_workflow.yml
git commit -m "Añadir job de linter"
git push -u origin nodejs_linter_job
~~~

## Paso 5. Revisar Actions
Abre el workflow y el log de Super-Linter. Si falla, revisa rama, configuración, archivos validados y reglas del linter.