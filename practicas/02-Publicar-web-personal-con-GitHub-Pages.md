# Práctica 2. Publicar una web personal con GitHub Pages

## Objetivo
Subir una página estática a GitHub y publicarla con una dirección del tipo `https://TU_USUARIO.github.io/`.

## 1. Crear el repositorio correcto
En GitHub crea un repositorio **público** llamado exactamente `TU_USUARIO.github.io`, sustituyendo el texto por tu nombre de usuario. Este formato identifica un sitio personal.

## 2. Preparar y comprobar los archivos
Abre una terminal y entra en la carpeta donde tengas los archivos de la web. Sustituye `carpeta-del-sitio` por la ruta real:

```bash
cd carpeta-del-sitio
pwd
ls -la
```

- `cd carpeta-del-sitio`: entra en la carpeta del proyecto.
- `pwd`: muestra la ruta actual para comprobar que estás en el lugar correcto.
- `ls -la`: lista los archivos, incluidos los ocultos.

Debe existir un archivo llamado exactamente `index.html` en la raíz que vas a publicar. Si el enunciado requiere crear una página mínima desde cero, puedes usar este ejemplo; si ya te han dado los archivos, conserva los del enunciado:

```bash
cat > index.html <<'EOF'
<!doctype html>
<html lang="es">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Mi web personal</title>
</head>
<body>
  <h1>Mi web personal</h1>
  <p>Mi primera página publicada con GitHub Pages.</p>
</body>
</html>
EOF
cat index.html
```

- `cat > index.html`: crea o sustituye el archivo con el contenido escrito hasta `EOF`.
- `cat index.html`: muestra el archivo para revisar que se ha guardado.

## 3. Inicializar Git y subir el proyecto
Ejecuta estos comandos **solo si la carpeta todavía no es un repositorio y no tiene remoto**:

```bash
git init
git branch -M main
git status
git add .
git status
git commit -m "Publicar sitio inicial"
git remote add origin https://github.com/TU_USUARIO/TU_USUARIO.github.io.git
git push -u origin main
```

- `git init`: inicializa Git.
- `git branch -M main`: establece el nombre de la rama como `main`.
- Primer `git status`: permite comprobar qué archivos existen y qué cambios detecta Git.
- `git add .`: prepara los archivos de la carpeta actual.
- Segundo `git status`: comprueba lo que se va a incluir en el commit.
- `git commit -m "Publicar sitio inicial"`: guarda una versión en el historial.
- `git remote add origin ...`: conecta el repositorio local con el repositorio de GitHub. Sustituye `TU_USUARIO` por tu usuario real.
- `git push -u origin main`: publica la rama `main` y configura el seguimiento del remoto.

Si el repositorio ya está clonado o el remoto ya existe, no repitas `git init` ni `git remote add origin`. Comprueba el remoto con `git remote -v`; después ejecuta `git add .`, `git commit -m "Actualizar sitio"` y `git push` según los cambios pendientes.

## 4. Activar Pages
En GitHub abre **Settings → Pages**. En la opción de origen selecciona **Deploy from a branch**, elige `main` y la carpeta `/(root)`, y guarda. GitHub procesará los archivos y publicará el sitio.

## 5. Verificar la web
Abre `https://TU_USUARIO.github.io/`, sustituyendo el usuario por el real. Comprueba:
- que carga `index.html`;
- que se ven los estilos;
- que las imágenes no aparecen rotas;
- que los enlaces funcionan;
- que la web se ve correctamente al cambiar el tamaño de la ventana.

## Errores frecuentes
- Si no aparece la web, revisa el nombre exacto del repositorio, la rama y la carpeta seleccionadas en Pages.
- Si aparece HTML sin diseño, comprueba la ruta del CSS.
- Si Git dice que `origin` ya existe, ejecuta `git remote -v` y no vuelvas a añadirlo.
- Los cambios pueden tardar un poco en publicarse.
