# Práctica 5. GitHub Flow: issues, ramas, pull requests y Pages

## Objetivo
Crear una issue, desarrollar en una rama, subir los cambios, abrir un pull request y publicar la web desde `docs/`.

## 1. Clonar el repositorio

```bash
git clone https://github.com/antoni-gimenez/gitFlow.git
cd gitFlow
git status
git remote -v
git branch --show-current
```

- `git clone ...`: descarga el repositorio y su historial en una carpeta llamada `gitFlow`.
- `cd gitFlow`: entra en la carpeta descargada.
- `git status`: comprueba el estado de la copia local.
- `git remote -v`: muestra la URL del repositorio remoto.
- `git branch --show-current`: muestra la rama actual.

Si el enunciado proporciona otra URL, utiliza esa URL en lugar del ejemplo. Si no tienes permisos de escritura, crea un fork o utiliza la copia autorizada que indique el profesor.

## 2. Crear una issue
En GitHub entra en **Issues → New issue**. Escribe un título claro, explica qué hay que cambiar y crea la issue. Anota el número real de la issue para vincularla al pull request.

## 3. Crear una rama para el cambio

```bash
git switch -c feature_nombre_alumno
git branch --show-current
git status
```

- `git switch -c feature_nombre_alumno`: crea y cambia a la nueva rama.
- `git branch --show-current`: comprueba que estás en esa rama.
- `git status`: comprueba que el árbol de trabajo está preparado para comenzar.

Sustituye `feature_nombre_alumno` por el nombre exacto indicado en el enunciado.

## 4. Revisar el contenido de docs
Desde la raíz del repositorio:

```bash
ls -la
ls -la docs
test -f docs/index.html && echo "index.html encontrado"
```

- `ls -la`: muestra el contenido de la raíz.
- `ls -la docs`: muestra los archivos que se publicarán desde `docs/`.
- `test -f docs/index.html ...`: comprueba que existe el archivo principal.

Si el enunciado pide crear una página de prueba y no hay una ya proporcionada, puedes usar:

```bash
mkdir -p docs
cat > docs/index.html <<'EOF'
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Práctica GitHub Flow</title>
</head>
<body>
  <h1>Práctica GitHub Flow</h1>
  <p>Página publicada desde la carpeta docs.</p>
</body>
</html>
EOF
cat docs/index.html
```

- `mkdir -p docs`: crea la carpeta si todavía no existe.
- `cat > docs/index.html ...`: crea la página HTML.
- `cat docs/index.html`: comprueba el contenido guardado.

Si el repositorio ya contiene la web del enunciado, no la sustituyas por esta página de ejemplo.

## 5. Editar, revisar y confirmar
Haz los cambios que solicite la issue y prueba el resultado. Después ejecuta:

```bash
git status
git diff
git add .
git status
git diff --staged
git commit -m "Añadir funcionalidad solicitada"
git push -u origin feature_nombre_alumno
```

- Primer `git status`: muestra los archivos modificados.
- `git diff`: enseña los cambios que todavía no están preparados.
- `git add .`: prepara los cambios de la carpeta actual.
- Segundo `git status`: comprueba qué archivos se incluirán en el commit.
- `git diff --staged`: permite revisar los cambios preparados.
- `git commit -m ...`: registra los cambios en el historial.
- `git push -u origin feature_nombre_alumno`: publica la rama y configura el seguimiento.

Si no hay cambios que confirmar, no crees un commit vacío. Si la rama tiene otro nombre, usa el mismo nombre en los comandos.

## 6. Abrir un Pull Request
En GitHub abre **Pull requests → New pull request**. Selecciona la rama de la funcionalidad como origen (*compare*) y `main` como destino (*base*), salvo que el enunciado diga otra cosa. Describe el cambio y vincula la issue con `Closes #NUMERO`, sustituyendo `NUMERO` por el número real. Solicita la revisión requerida y corrige las observaciones antes de integrar el cambio.

## 7. Publicar desde docs
Después de integrar el cambio en la rama que se publicará, abre **Settings → Pages**:
1. Selecciona **Deploy from a branch**.
2. Elige la rama indicada por el profesor, normalmente `main`.
3. Selecciona la carpeta `/docs`.
4. Guarda la configuración y espera al despliegue.

## 8. Comprobaciones para entregar
- Comprueba que el pull request está integrado.
- Comprueba que la issue se ha cerrado cuando corresponda.
- Abre la URL de Pages y revisa que la web se ve correctamente.
- Guarda las capturas que pida el enunciado.
- No hagas push directo a ramas protegidas ni cierres una issue sin comprobar el resultado.
