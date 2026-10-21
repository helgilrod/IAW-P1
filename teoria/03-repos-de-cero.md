# Repositorios de cero: GitHub web + terminal

## Dos maneras de empezar
Las dos acaban igual: una carpeta en tu ordenador conectada a un repositorio de GitHub. La diferencia es **dónde nace primero**.

| | Clonar | Conectar una carpeta |
|---|---|---|
| Qué existe primero | El repositorio en GitHub | Los ficheros en tu ordenador |
| Al crearlo en GitHub | **Con** README | **Sin** README |
| En la terminal | git clone URL y cd a la carpeta | git init, git remote add origin URL y git branch -M main |
| Primer push | git push | git push -u origin main |
| Cuándo | Empiezas de cero | Ya tienes los ficheros |

Como en Drive: clonar es descargarte una carpeta compartida que ya existe; conectar es subir una carpeta que tienes a una carpeta vacía.

En el examen: «crea el repositorio en GitHub y clónalo» es la primera; «crea una carpeta, inicializa Git y súbela» es la segunda. Si te dejan elegir, clonar tiene menos pasos y menos errores.

## Crear un repositorio en GitHub (web)
1. Botón **+** (arriba a la derecha) → **New repository**.
2. **Repository name**: el nombre exacto que pidan (mayúsculas y guiones cuentan).
3. **Public** o **Private**, según el enunciado.
4. **Add a README file**: márcalo si vas a clonarlo; déjalo sin marcar (y sin .gitignore ni licencia) si vas a conectar una carpeta que ya tienes.
5. **Create repository** y copia la URL HTTPS del botón **Code**.

## Manera 1: clonar
~~~bash
cd ~
git clone https://github.com/USUARIO/practica-git.git
cd practica-git
nano README.md
git add README.md
git commit -m "Actualizar README"
git push
~~~
- git clone crea la carpeta con el nombre del repositorio, hace el git init, conecta origin y se baja los commits. Por eso aquí no hacen falta git init ni git remote add.
- Recuerda el cd: hasta que no entras en la carpeta, no estás en el repositorio.
- El push funciona directamente, sin -u: la rama ya sabe a dónde sube.
- Si el repositorio ya está en tu ordenador, no lo clones otra vez: trae lo último con git pull.

## Manera 2: conectar una carpeta que ya tienes
~~~bash
mkdir -p ~/practica-git
cd ~/practica-git
git init
git remote add origin https://github.com/USUARIO/practica-git.git
git branch -M main
git remote -v
~~~
- mkdir -p crea la carpeta y no da error si ya existe.
- git branch -M main renombra la rama actual a main (GitHub espera main).
- git remote -v comprueba que origin apunta a la URL correcta.
- Después creas los ficheros, haces el commit y el primer push con git push -u origin main (lo tienes en los apartados siguientes). El push necesita al menos un commit hecho.
- Por qué el repositorio va sin README: si GitHub tiene un commit que tú no tienes, son dos historias que no encajan y el primer push se rechaza (*fetch first*).

## Crear ficheros desde la terminal
~~~bash
echo "# Mis notas" > notas.md
echo "Segunda línea" >> notas.md
cat notas.md
nano notas.md
~~~
- > crea el fichero o lo **sobrescribe**; >> añade al final.
- cat muestra el contenido.
- nano: pega con Ctrl+Shift+V, guarda con Ctrl+O y Enter, sal con Ctrl+X.

Para varias líneas (por ejemplo, HTML) también sirve un heredoc:
~~~bash
cat > inicio.html <<'EOF'
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8">
<title>Página de prueba</title>
</head>
<body>
<h1>Hola</h1>
</body>
</html>
EOF
~~~
Con 'EOF' entre comillas, el texto se escribe tal cual, sin interpretar variables.

## Guardar la salida de un comando en un fichero
~~~bash
git log --oneline > historial.txt
ls -la > listado.txt
~~~
Es más seguro que copiar y pegar: no se pierde ninguna línea. El fichero recoge el historial del momento en que ejecutas el comando; el commit en el que luego lo añades no sale dentro.

## Un commit con un solo fichero
~~~bash
git add notas.md
git status
git commit -m "Mensaje exacto"
git log --oneline
~~~
- git status: en verde lo que entra en el commit; en rojo lo que no.
- El mensaje debe ser exactamente el que pidan (mayúsculas, tildes y espacios).
- Con git add . entra todo; si solo debe ir un fichero, nómbralo.

## Subir los commits
~~~bash
git push -u origin main
git push
~~~
- La primera vez, -u deja configurado que main sube a origin; después basta con git push.
- Por HTTPS, GitHub no acepta la contraseña de la cuenta: usa un token personal (Settings → Developer settings → Personal access tokens) como contraseña, o inicia sesión con gh auth login.

## Descargar y descomprimir un ZIP de GitHub
- Repositorio entero: botón verde **Code** → **Download ZIP**. La carpeta que sale se llama nombre-del-repo-main.
- Un .zip que está dentro de un repositorio: abre el fichero y pulsa **Download raw file**.

~~~bash
cd ~/Descargas
ls
unzip practica.zip
ls
cd practica
~~~
- Usa ls para ver el nombre real de la carpeta antes de entrar.
- Si unzip no existe: sudo apt install unzip. En Windows: clic derecho → Extraer todo.

## Subir archivos desde la web
**Add file** → **Upload files** → arrastra el **contenido** de la carpeta (no el .zip) → **Commit changes**. Si arrastras carpetas, GitHub mantiene su estructura.

## Borrar un repositorio
Settings → General → **Danger Zone** → **Delete this repository**, y escribe el nombre para confirmar. No se puede deshacer.

## Repositorio dentro de otro
Si haces git init en una carpeta que está dentro de otro repositorio, quedan dos repositorios anidados. Si en el de fuera haces git add ., Git avisa de un *embedded repository* y no sube su contenido. Añade solo los ficheros que toquen en cada repositorio, o ignora la carpeta interior en el .gitignore del de fuera.

## Errores típicos
| Mensaje | Causa | Solución |
|---|---|---|
| remote origin already exists | Ya habías añadido el remoto | git remote set-url origin URL |
| src refspec main does not match any | No hay commits, o la rama no se llama main | Haz un commit; git branch -M main |
| rejected ... (fetch first) | El remoto tiene commits que tú no (README creado en la web) | git pull --rebase origin main y vuelve a hacer push |
| You appear to have cloned an empty repository | Has clonado un repositorio creado sin README | No es un error: crea un fichero, haz commit, git branch -M main y git push -u origin main |
| Authentication failed | Has usado la contraseña en vez de un token | Usa un token o gh auth login |
| not a git repository | No estás en la carpeta del repositorio | cd a la carpeta; ls -a debe mostrar .git |

## Comprueba que sabes
- [ ] Explicar cuándo se clona y cuándo se conecta una carpeta, y por qué cambia lo del README.
- [ ] Crear un repositorio con README en GitHub y clonarlo.
- [ ] Crear un repositorio vacío en GitHub y conectarlo con una carpeta local.
- [ ] Crear un fichero con echo y otro con nano.
- [ ] Guardar la salida de git log --oneline en un fichero.
- [ ] Hacer un commit con un solo fichero y un mensaje exacto.
- [ ] Hacer el primer push y resolver un rechazo por fetch first.
- [ ] Descargar un ZIP, descomprimirlo y entrar en la carpeta.

---
[← 02 .gitignore](02-gitignore.md) · [Índice](../README.md) · [04 Ramas, merge y tags →](04-ramas-merge-tags.md)
