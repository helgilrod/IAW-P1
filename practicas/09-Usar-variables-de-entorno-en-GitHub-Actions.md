# Práctica 9. Usar una variable de entorno en GitHub Actions

## Objetivo
Definir un dato una vez y utilizarlo en un paso del workflow.

## 1. Crear la rama
```bash
git switch -c nodejs_environment_job
```

## 2. Crear `.github/workflows/environment.yml`
```yaml
name: Environment test
on:
  push:
    branches: [nodejs_environment_job]
  workflow_dispatch:
jobs:
  environment_test_execution:
    runs-on: ubuntu-latest
    env:
      DESTINATION_PERSON: "2DAW"
    steps:
      - name: Saludar
        run: echo "Hello $DESTINATION_PERSON"
```

## 3. Entender la configuración
- `env` define variables de entorno. Aquí está al nivel del job, por lo que se aplica a sus pasos.
- `DESTINATION_PERSON` es el nombre de la variable; `2DAW` es su valor.
- `run` ejecuta un comando de shell en el runner Ubuntu.
- En un shell Linux, se lee la variable con `$DESTINATION_PERSON`; la salida esperada es `Hello 2DAW`.
- `workflow_dispatch` permite lanzar la práctica manualmente desde la pestaña Actions.

## 4. Subir y comprobar
```bash
git add .github/workflows/environment.yml
git commit -m "Añadir variable de entorno"
git push -u origin nodejs_environment_job
```
En GitHub abre **Actions**, selecciona la ejecución y revisa el log del paso «Saludar». Si la salida está vacía, comprueba la indentación YAML y el nivel de `env`.

## Si el enunciado pide una acción de saludo
El material original también menciona una acción externa con un parámetro como `who-to-greet`. Si se evalúa el uso de esa acción, conserva su nombre, versión e inputs exactos y pasa la variable con `with`; el ejemplo de `echo` solo demuestra el mecanismo de variable.

## Seguridad
No incluyas contraseñas ni tokens como valores literales en el repositorio. Para credenciales utiliza GitHub Secrets y referencia el secreto desde el workflow.