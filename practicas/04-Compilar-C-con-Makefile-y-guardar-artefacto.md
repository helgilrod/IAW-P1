# Práctica 4. Compilar un programa C con Makefile y GitHub Actions

## Objetivo
Automatizar la compilación de C, ejecutar una prueba sencilla y guardar el binario generado como artefacto descargable.

**Teoría:** [.gitignore](../teoria/02-gitignore.md) · [GitHub Actions y YAML](../teoria/05-actions-yaml.md) · [Ejemplos de workflows](../teoria/07-ejemplos-actions.md)

## 1. Crear el programa
En la raíz crea `estadistiques.c`:
```c
#include <stdio.h>

int main(void) {
    printf("Programa estadistiques\n");
    return 0;
}
```
`stdio.h` permite usar `printf`. La función `main` es el punto de entrada y devolver `0` indica que terminó correctamente.

## 2. Crear el Makefile
Crea un archivo llamado exactamente `Makefile`:
```makefile
CC = gcc
CFLAGS = -Wall -Wextra -Werror

all: estadistiques

estadistiques: estadistiques.c
	$(CC) $(CFLAGS) estadistiques.c -o estadistiques

test: estadistiques
	./estadistiques

clean:
	rm -f estadistiques
```
**Importante:** las líneas de comandos bajo cada regla deben empezar con una tabulación real, no con espacios. `all` es el objetivo predeterminado; `test` ejecuta el programa y `clean` elimina el binario.

## 3. Evitar subir el binario
En `.gitignore` añade:
```gitignore
*.o
estadistiques
```
Así Git ignora objetos compilados y el ejecutable, pero sigue versionando el código fuente y el Makefile.

Compruébalo después de compilar:
```bash
make
git check-ignore -v estadistiques
git status
```
`git check-ignore -v` indica la línea del `.gitignore` que ignora el binario; si no imprime nada, no se está ignorando. En `git status` no debe aparecer `estadistiques`.

## 4. Crear el workflow
Guarda esto en `.github/workflows/compilar.yml`:
```yaml
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
```
El runner Ubuntu ya dispone de herramientas habituales de compilación. La subida de artefacto conserva el archivo al terminar el job, aunque esté ignorado por Git.

## 5. Confirmar y verificar
```bash
git add estadistiques.c Makefile .gitignore .github/workflows/compilar.yml
git commit -m "Añadir compilación y pruebas de C"
git push
```
En **Actions**, comprueba por separado que se detecta GCC, que `make` compila, que `make test` devuelve código cero y que el artefacto se puede descargar.

## Diagnóstico
Un error de sintaxis detiene la compilación. Una tabulación incorrecta produce errores de Makefile. Si falla la subida, verifica que el nombre del archivo coincide con `path`. Puedes introducir un error controlado para ver el fallo y luego corregirlo. Si el enunciado pide un badge, añádelo al README usando la opción de estado del workflow.

---
[← Práctica 3](03-Compilar-Python-con-PyInstaller-en-tres-sistemas.md) · [Índice de prácticas](README.md) · [Práctica 5 →](05-Trabajar-con-issues-ramas-pull-requests-y-Pages.md)
