# Práctica 4. Compilar un programa C con Makefile y GitHub Actions

## Objetivo
Automatizar la compilación de C, ejecutar una prueba sencilla y guardar el binario generado como artefacto descargable.

## 1. Comprobar la carpeta de trabajo

```bash
pwd
ls -la
mkdir -p .github/workflows
```

- `pwd`: muestra la carpeta actual.
- `ls -la`: permite revisar el contenido del proyecto.
- `mkdir -p .github/workflows`: crea la ruta para el workflow.

## 2. Crear el programa C
Crea `estadistiques.c` con este comando:

```bash
cat > estadistiques.c <<'EOF'
#include <stdio.h>

int main(void) {
    printf("Programa estadistiques\\n");
    return 0;
}
EOF
cat estadistiques.c
```

- `cat > estadistiques.c`: crea el archivo con el código.
- `cat estadistiques.c`: comprueba el contenido guardado.
- `stdio.h` permite usar `printf`.
- `main` es el punto de entrada del programa y devolver `0` indica que terminó correctamente.

## 3. Crear el Makefile
El archivo debe llamarse exactamente `Makefile`:

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

- `cat > Makefile`: crea el archivo.
- `cat -te Makefile`: permite distinguir las tabulaciones; las líneas de comandos bajo cada regla deben empezar con una tabulación real.
- `all` es el objetivo predeterminado.
- `test` ejecuta el programa.
- `clean` elimina el binario.

## 4. Probar la compilación local
En un sistema con GCC y Make instalados:

```bash
gcc --version
make
make test
ls -l estadistiques
make clean
```

- `gcc --version`: muestra la versión del compilador.
- `make`: compila el programa siguiendo el objetivo predeterminado.
- `make test`: ejecuta el programa compilado.
- `ls -l estadistiques`: comprueba que existe el ejecutable.
- `make clean`: elimina el ejecutable generado.

## 5. Evitar subir el binario
Añade estas reglas a `.gitignore`. Si el archivo no existe, el siguiente comando lo crea; si ya existe, revisa su contenido antes de sustituirlo:

```bash
cat >> .gitignore <<'EOF'
*.o
estadistiques
EOF
cat .gitignore
```

- `cat >> .gitignore`: añade las dos reglas al final del archivo.
- `*.o`: ignora archivos objeto.
- `estadistiques`: ignora el ejecutable compilado.

## 6. Crear el workflow
Ejecuta el bloque en la raíz del repositorio:

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
      - uses: actions/checkout@v4
      - name: Versión de GCC
        run: gcc --version
      - name: Compilar
        run: make
      - name: Prueba básica
        run: make test
      - name: Subir ejecutable
        uses: actions/upload-artifact@v4
        with:
          name: estadistiques
          path: estadistiques
          retention-days: 7
EOF
cat .github/workflows/compilar.yml
```

- `cat > ... <<'EOF'`: escribe el workflow en la ruta indicada.
- `cat ...`: permite revisar el YAML antes de subirlo.
- El artefacto conserva el ejecutable al terminar el job, aunque esté ignorado por Git.

## 7. Confirmar y verificar

```bash
git status
git add estadistiques.c Makefile .gitignore .github/workflows/compilar.yml
git diff --staged
git commit -m "Añadir compilación y pruebas de C"
git push
```

- `git status`: revisa los cambios.
- `git add ...`: prepara los cuatro archivos.
- `git diff --staged`: revisa los cambios preparados.
- `git commit ...`: crea el commit.
- `git push`: sube los cambios a la rama configurada.

En **Actions**, comprueba que GCC se detecta, que `make` compila, que `make test` termina correctamente y que el artefacto se puede descargar.

## Diagnóstico
- Un error de sintaxis detiene la compilación.
- Una tabulación incorrecta produce errores de Makefile.
- Si falla la subida, verifica que el archivo generado coincide con `path`.
- Si el enunciado pide un badge, añádelo al README usando la URL de estado del workflow.
