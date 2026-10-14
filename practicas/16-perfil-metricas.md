# Práctica 16. Métricas del perfil GitHub

## Paso 1. Crear o abrir el repositorio
Usa el repositorio de perfil indicado por el profesor, normalmente el repositorio especial del usuario o el repositorio personal solicitado.

## Paso 2. Preparar README
Crea README.md con una sección para mostrar las métricas de lenguajes. La acción puede generar una imagen/SVG que luego se inserta en el README según su documentación.

## Paso 3. Crear workflow
Guarda el workflow en .github/workflows/metrics.yml. La actividad original pide una acción de métricas; el nombre y sus parámetros exactos deben copiarse del enunciado porque no están definidos en el resumen disponible.

Estructura orientativa, con marcador que debes sustituir:
~~~yaml
name: Actualizar métricas
on:
  schedule:
    - cron: "0 0 * * *"
  workflow_dispatch:
jobs:
  metrics:
    runs-on: ubuntu-latest
    steps:
      - name: Generar métricas
        uses: ACCION_DE_METRICAS_INDICADA_POR_EL_PROFESOR
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
~~~
ACCION_DE_METRICAS_INDICADA_POR_EL_PROFESOR es un marcador, no una acción ejecutable.

## Paso 4. Permisos y secretos
Configura solo los permisos necesarios. Si la acción necesita un token personal, guárdalo en Settings → Secrets and variables → Actions y usa el nombre del secreto en YAML.

## Paso 5. Ejecutar y verificar
Lanza el workflow manualmente, revisa los logs y comprueba que el README muestra la imagen de métricas. Añade las capturas que solicite el profesor.