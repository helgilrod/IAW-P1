# Práctica 5. GitHub Flow y Pages desde docs

## Paso 1. Clonar
~~~bash
git clone https://github.com/antoni-gimenez/gitFlow.git
cd gitFlow
git status
~~~
Trabaja en tu fork/copia autorizada si no tienes permisos para subir al repo original.

## Paso 2. Crear issue
En GitHub abre Issues → New issue, describe la tarea y crea la incidencia. Anota su número.

## Paso 3. Crear rama
~~~bash
git switch -c feature_nombre_alumno
~~~
Usa el nombre exacto del enunciado.

## Paso 4. Modificar y confirmar
Haz el cambio, pruébalo y revisa:
~~~bash
git status
git diff
git add .
git commit -m "Añadir funcionalidad solicitada"
git push -u origin feature_nombre_alumno
~~~

## Paso 5. Pull request
En GitHub abre Pull requests → New pull request. Elige la rama de funcionalidad como compare y main como base. Explica el cambio, vincula la issue con Closes #NUMERO, asigna el revisor y solicita revisión. Corrige observaciones y haz merge cuando esté aprobado.

## Paso 6. Pages desde docs
Comprueba que docs/index.html existe. En Settings → Pages selecciona la rama indicada (normalmente main) y carpeta /docs.

## Paso 7. Verificar y documentar
Abre la URL de Pages, comprueba la web y guarda las capturas pedidas. No hagas push directo a ramas protegidas.