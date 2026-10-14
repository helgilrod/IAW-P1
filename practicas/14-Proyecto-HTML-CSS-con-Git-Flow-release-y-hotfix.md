# Práctica 14. Proyecto HTML/CSS con Git Flow, release y hotfix

## Objetivo
Desarrollar una web por etapas usando ramas diferenciadas, integrar los cambios, preparar la versión `v1.0` y practicar una corrección urgente.

## 1. Clonar el repositorio
```bash
git clone URL_DEL_REPOSITORIO
cd nombre-repositorio
git status
```
Sustituye los marcadores por los valores del enunciado. Comprueba que estás en el repositorio correcto y que no hay cambios pendientes.

## 2. Crear el contenido HTML
```bash
git switch -c feature/contingutHTML
```
Construye cabecera, navegación, contenido principal y pie. Revisa el anidamiento de etiquetas y los enlaces.
```bash
git add .
git commit -m "Añadir contenido HTML"
git push -u origin feature/contingutHTML
```

## 3. Añadir atributos HTML
Desde la rama base indicada por el profesor crea `feature/atributsHTML`. Incorpora los atributos que solicite el ejercicio, como enlaces, imágenes, identificadores o texto alternativo. Prueba el resultado antes de confirmar.

## 4. Añadir CSS
Crea `feature/estilsCSS` desde la base que corresponda. Crea el archivo CSS y enlázalo desde el HTML. Abre la página en un navegador para comprobar tipografía, márgenes, colores y distribución; luego confirma y sube la rama.

## 5. Integrar las ramas
Integra cada funcionalidad siguiendo el diagrama de Git Flow de clase. Si aparece un conflicto, abre los archivos afectados, decide qué contenido conservar, elimina los marcadores `<<<<<<<`, `=======` y `>>>>>>>`, y prueba antes de completar la integración.

## 6. Preparar release v1.0
Crea la rama de release con el nombre indicado por el profesor. En ella se realizan ajustes finales, pruebas y preparación de la versión. Respeta el flujo de clase sobre qué ramas reciben la release.

## 7. Crear un hotfix
Desde la versión estable crea `hotfix/milloresV_1_0` o el nombre exacto del enunciado. Corrige el problema urgente, prueba y confirma. Integra la corrección tanto en la rama estable como en desarrollo cuando el flujo de clase lo requiera.

## 8. Publicar y documentar
Configura GitHub Pages con la rama o carpeta que solicite el ejercicio. Si se pide invitar a `antoni-gimenez`, crear un proyecto/canvas o añadir capturas, hazlo en GitHub con los permisos adecuados.

## 9. Comprobación final
Verifica HTML, CSS, navegación, nombres de ramas, integraciones, release, hotfix y URL publicada. Añade la documentación y capturas solicitadas. No hagas push a ramas protegidas sin permiso.