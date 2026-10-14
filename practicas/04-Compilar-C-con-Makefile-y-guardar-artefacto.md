# Práctica 4. Compilar un programa C con Makefile y guardar el artefacto

## Objetivo
Crear un programa C, compilarlo con un Makefile, ejecutar una prueba y automatizar la compilación con GitHub Actions. El binario se guardará como artefacto.

## 1. Comprobar el proyecto

Desde la raíz del repositorio:

```bash
pwd
ls -la
git status
mkdir -p .github/workflows
```

- `pwd`: muestra la ruta actual.
- `ls -la`: lista los archivos del proyecto.
- `git status`: muestra la rama y los cambios pendientes.
- `mkdir -p .github/workflows`: crea la carpeta de workflows.

## 2. Crear el programa C

```bash
cat > estadistiques.c <<'EOF'
#include <stdio.h>

int main(void) {
    printf("Programa estadistiques\n");
    return 0;
}
EOF
cat estadistiques.c
```

- `cat > estadistiques.c`: crea el archivo con el código C.
- `#include <stdio.h>`: permite utilizar funciones de entrada/salida como `printf`.
- `main`: es el punto de entrada del programa.
- `printf`: muestra el texto por pantalla.
- `return 0`: indica que el programa ha terminado correctamente.
- `cat estadistiques.c`: comprueba que el código se ha guardado.

## 3. Crear el Makefile

El archivo se debe llamar exactamente `Makefile`, respetando la M mayúscula. Las líneas de comandos debajo de cada regla deben empezar con una tabulación real.

```bash
cat > Makefile <<'EOF'
CC = gcc
CFLAGS = -Wall -Wextra -Werror

all: estadistiques

estadistiques: estadistiques.c
	$(CC) $(CFLAGS) estadistiques.c -o estadistiques

test: estadistiques
	./estadistiques

clean:
	rm -f estadistiques
EOF
cat -te Makefile
```

- `CC = gcc`: define el compilador.
- `CFLAGS`: activa avisos y trata los avisos como errores.
- `all`: objetivo predeterminado.
- `estadistiques: estadistiques.c`: indica que el ejecutable depende del código fuente.
- La línea `$(CC)...` compila el archivo y genera el ejecutable.
- `test`: ejecuta el programa compilado.
- `clean`: elimina el ejecutable generado.
- `cat -te`: permite detectar tabulaciones; las órdenes de Makefile deben empezar por tabulación, no por espacios.

## 4. Probar localmente

En Linux con GCC y Make instalados:

```bash
gcc --version
make
make test
ls -l estadistiques
make clean
```

- `gcc --version`: muestra la versión de GCC.
- `make`: ejecuta el objetivo predeterminado y compila.
- `make test`: ejecuta el programa; debería imprimir `Programa estadistiques`.
- `ls -l estadistiques`: comprueba que el ejecutable se creó.
- `make clean`: borra el ejecutable para limpiar el proyecto.

Si `make` o `gcc` no están instalados en Debian/Ubuntu, el profesor puede pedir instalarlos con `sudo apt update` y `sudo apt install build-essential`.

## 5. Crear o actualizar .gitignore

Si `.gitignore` ya existe, revisa su contenido antes de modificarlo. Para añadir las reglas al final:

```bash
cat >> .gitignore <<'EOF'
*.o
estadistiques
EOF
cat .gitignore
```

- `cat >>`: añade contenido sin borrar las reglas existentes.
- `*.o`: ignora archivos objeto.
- `estadistiques`: evita que el ejecutable local se suba al repositorio.
- El binario podrá subirse como artefacto desde Actions aunque Git lo ignore.

## 6. Crear el workflow de GitHub Actions

```bash
cat > .github/workflows/compilar.yml <<'EOF'
name: Compilar C
on:
  push:
    branches: [main]
  workflow_dispatch:

jobs:
  compilar:
    runs-on: ubuntu-latest
    steps:
      - name: Descargar repositorio
        uses: actions/checkout@v4
      - name: Mostrar versión de GCC
        run: gcc --version
      - name: Compilar
        run: make
      - name: Ejecutar prueba
        run: make test
      - name: Guardar ejecutable como artefacto
        uses: actions/upload-artifact@v4
        with:
          name: estadistiques-linux
          path: estadistiques
          retention-days: 7
EOF
cat .github/workflows/compilar.yml
```

- `on.push.branches`: ejecuta el workflow al hacer push a `main`.
- `workflow_dispatch`: permite iniciarlo manualmente.
- `runs-on: ubuntu-latest`: ejecuta el job en Linux.
- `actions/checkout`: descarga el repositorio.
- `run: make`: compila usando el Makefile.
- `run: make test`: comprueba que el programa se ejecuta.
- `actions/upload-artifact`: permite descargar el ejecutable al acabar el workflow.
- `retention-days: 7`: conserva el artefacto siete días.

## 7. Revisar y subir los archivos

```bash
git status
git add estadistiques.c Makefile .gitignore .github/workflows/compilar.yml
git status
git diff --staged
git commit -m "Añadir compilacion y pruebas de C"
git push
```

- Primer `git status`: muestra los cambios.
- `git add ...`: prepara los archivos de código, configuración y workflow.
- Segundo `git status`: confirma qué se va a incluir en el commit.
- `git diff --staged`: revisa los cambios de texto preparados.
- `git commit`: guarda la versión.
- `git push`: sube los cambios a la rama configurada.

## 8. Verificar el resultado

1. En GitHub, abre **Actions**.
2. Selecciona la ejecución de **Compilar C**.
3. Comprueba que compilar y ejecutar la prueba terminan correctamente.
4. Descarga el artefacto `estadistiques-linux`.
5. Si falla, abre el log del paso rojo y revisa el error exacto.

## Errores frecuentes

- **Missing separator:** una orden del Makefile empieza con espacios en lugar de tabulación.
- **No such file:** el nombre del fuente o el ejecutable no coincide.
- **No aparece el artefacto:** el nombre de `path` no coincide con el binario generado.
- **No se inicia Actions:** el archivo debe estar en `.github/workflows/` y los cambios deben haberse subido a la rama indicada.
