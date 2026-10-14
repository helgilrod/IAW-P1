# Práctica 16. Automatizar las métricas del perfil de GitHub

## Objetivo
Crear un workflow que genere o actualice una imagen de métricas y mostrarla en el README del perfil. La acción exacta y sus parámetros deben ser los del enunciado.

## 1. Abrir el repositorio correcto
Usa el repositorio de perfil indicado por el profesor. Habitualmente el repositorio especial de perfil tiene el mismo nombre que el usuario de GitHub, pero confirma ese requisito antes de crear uno nuevo.

## 2. Preparar el README
Crea o edita `README.md` y reserva una sección para la imagen de métricas. Muchas acciones generan un SVG u otro archivo gráfico que se incluye con Markdown:
```markdown
![Métricas del perfil](RUTA_DE_LA_IMAGEN_GENERADA)
```
`RUTA_DE_LA_IMAGEN_GENERADA` es un marcador: sustitúyelo por la ruta real que produzca la acción.

## 3. Crear el workflow
Guarda el archivo en `.github/workflows/metrics.yml`. Estructura orientativa:
```yaml
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
```
- `schedule` permite ejecutar el workflow automáticamente según una expresión cron; en el ejemplo, diariamente a medianoche UTC.
- `workflow_dispatch` permite lanzarlo manualmente desde Actions.
- `runs-on` selecciona el runner.
- `uses` identifica la acción externa.
- `with` pasa los parámetros que esa acción necesita.

El nombre de acción del ejemplo es un marcador, no una acción ejecutable. Copia el nombre, la versión y los inputs del material de clase.

## 4. Revisar permisos y tokens
Configura solo los permisos necesarios. Si la acción requiere un token personal, guárdalo en **Settings → Secrets and variables → Actions** y referencia el secreto desde el YAML. Nunca pegues tokens directamente en el workflow, README o capturas públicas.

## 5. Ejecutar y comprobar
Sube el workflow y ejecútalo manualmente. Abre el log del job y comprueba que se ha generado la imagen o archivo en la ruta esperada. Después, verifica que el README muestra la imagen. Si la acción modifica un archivo del repositorio, puede necesitar permisos de escritura y un paso para publicar el cambio.

## Errores habituales
- El workflow no aparece: revisa que esté en `.github/workflows/` y tenga extensión YAML.
- La acción falla: revisa nombre, versión, inputs y permisos.
- La imagen no aparece: comprueba que el archivo exista y que la ruta del README sea correcta.
- No se actualiza automáticamente: revisa el cron y que Actions esté habilitado.

## Para entregar
Sustituye los marcadores por los valores reales del enunciado, comprueba la ejecución y añade las capturas que solicite el profesor.