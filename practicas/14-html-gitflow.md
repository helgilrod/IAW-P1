# Práctica 14. Proyecto HTML/CSS con flujo Git

## Paso 1. Clonar el repositorio de clase
~~~bash
git clone URL_DEL_REPOSITORIO
cd nombre-repositorio
git status
~~~

## Paso 2. Rama de contenido HTML
~~~bash
git switch -c feature/contingutHTML
~~~
Crea cabecera, navegación, contenido y pie. Comprueba la página y sube:
~~~bash
git add .
git commit -m "Añadir contenido HTML"
git push -u origin feature/contingutHTML
~~~

## Paso 3. Rama de atributos HTML
Vuelve a la rama base indicada por el profesor y crea feature/atributsHTML. Añade los atributos que pida el ejercicio (enlaces, imágenes, identificadores, accesibilidad), prueba y confirma.

## Paso 4. Rama de CSS
Crea feature/estilsCSS desde la rama base que corresponda. Añade el CSS y enlázalo desde HTML. Comprueba en navegador y confirma.

## Paso 5. Integrar funcionalidades
Integra las ramas siguiendo el flujo Git Flow explicado en clase. Resuelve conflictos, prueba la web y conserva el historial.

## Paso 6. Release v1.0
Crea la rama de release según el esquema de clase, completa las pruebas y prepara la versión v1.0. Usa el nombre exacto del enunciado.

## Paso 7. Hotfix
Crea hotfix/milloresV_1_0 desde la versión estable, corrige el problema y confirma. Integra la corrección en la rama estable y en desarrollo según el flujo de clase.

## Paso 8. Pages y documentación
Configura gh-pages como indique el ejercicio. Invita a antoni-gimenez si el profesor lo solicita, crea el proyecto/canvas pedido y documenta teoría con capturas.

## Paso 9. Verificar
Comprueba navegación, estilos, enlaces, ramas, release, hotfix y URL publicada. No hagas push a ramas protegidas sin permisos.