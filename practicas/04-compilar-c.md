# Práctica 4. C, Makefile, pruebas y artefacto

## Paso 1. Crear archivos
En la raíz crea estadistiques.c, Makefile, .gitignore y .github/workflows/compilar.yml.

~~~c
#include <stdio.h>
int main(void) {
    printf("Programa estadistiques\n");
    return 0;
}
~~~

## Paso 2. Makefile
Los comandos de las reglas deben comenzar con una tabulación real.
~~~makefile
CC = gcc
CFLAGS = -Wall -Wextra -Werror

all: estadistiques

estadistiques: estadistiques.c
	$(CC) $(CFLAGS) estadistiques.c -o estadistiques

test: estadistiques
	./estadistiques

clean:
	rm -f estadistiques
~~~

## Paso 3. .gitignore
~~~gitignore
*.o
estadistiques
~~~
No ignores el código fuente ni el Makefile.

## Paso 4. Workflow
Archivo .github/workflows/compilar.yml:
~~~yaml
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
~~~

## Paso 5. Subir
~~~bash
git add estadistiques.c Makefile .gitignore .github/workflows/compilar.yml
git commit -m "Añadir compilación y pruebas de C"
git push
~~~

## Paso 6. Comprobar y practicar fallos
En Actions, GCC debe detectarse, make compilar, make test devolver código 0 y el artefacto quedar disponible. Introduce un error de sintaxis controlado, observa el fallo y corrígelo. Si falla la subida, revisa que path coincida con el ejecutable. Si el enunciado pide badge, copia el badge desde Actions al README.