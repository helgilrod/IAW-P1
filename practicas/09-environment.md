# Práctica 9. Variable de entorno

## Paso 1. Crear rama
~~~bash
git switch -c nodejs_environment_job
~~~

## Paso 2. Crear .github/workflows/environment.yml
~~~yaml
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
~~~

La actividad original también utiliza una acción externa de saludo con el parámetro who-to-greet. Si el enunciado pide esa acción, usa la acción y versión exactas indicadas y pasa DESTINATION_PERSON mediante with.

## Paso 3. Subir
~~~bash
git add .github/workflows/environment.yml
git commit -m "Añadir variable de entorno"
git push -u origin nodejs_environment_job
~~~

## Paso 4. Comprobar
En Actions debe aparecer Hello 2DAW. Una variable de entorno no es un secreto y no debe usarse para credenciales privadas.