# Práctica 6. Analizar JavaScript con Super-Linter

## Objetivo
Añadir un workflow que ejecute Super-Linter al subir cambios a una rama concreta. Un linter analiza el código para detectar problemas de formato, estilo y determinadas reglas de calidad; no sustituye las pruebas funcionales.

## 1. Crear o cambiar a la rama
```bash
git switch -c nodejs_linter_job
```
Si la rama ya existe, usa `git switch nodejs_linter_job`. Confirma que estás en la rama correcta con `git branch --show-current`.

## 2. Preparar la configuración
Comprueba que el repositorio contiene los archivos JavaScript y el archivo de configuración que espera el ejercicio, por ejemplo `.eslintrc.json`. La ruta del archivo de reglas debe coincidir con la estructura real del repositorio.

## 3. Crear el workflow
Guarda como `.github/workflows/main_workflow.yml`:
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
      - name: Linter
        uses: github/super-linter@v4
        env:
          DEFAULT_BRANCH: nodejs_example
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          LINTER_RULES_PATH: /
          JAVASCRIPT_ES_CONFIG_FILE: .eslintrc.json
          VALIDATE_JAVASCRIPT_ES: true
```
Este ejemplo conserva las versiones de la práctica. `on.push.branches` limita cuándo se ejecuta; `runs-on` elige el runner; `checkout` descarga el repositorio; y las variables `env` configuran el linter. `GITHUB_TOKEN` es un token automático proporcionado por Actions, no una contraseña que debas pegar en el archivo.

## 4. Confirmar y subir
```bash
git add .github/workflows/main_workflow.yml
git commit -m "Añadir job de linter"
git push -u origin nodejs_linter_job
```

## 5. Revisar el resultado
Abre **Actions**, entra en el workflow y examina el log. Si falla, comprueba que el workflow se ejecuta en la rama correcta, que la configuración existe en la ruta esperada y que las reglas se corresponden con el código.

## Nota
La versión v4 de Super-Linter y sus variables pertenecen al material de esta práctica; si el profesor exige conservarlas, no las actualices por tu cuenta durante la entrega.