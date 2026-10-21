# Tests de Python en GitHub Actions

## unittest en un minuto
~~~python
# calculadora.py
def suma(a, b):
    return a + b
~~~
~~~python
# test_calculadora.py
import unittest
from calculadora import suma


class TestSuma(unittest.TestCase):
    def test_positivos(self):
        self.assertEqual(suma(2, 3), 5)

    def test_negativos(self):
        self.assertEqual(suma(-1, -1), -2)
~~~
~~~bash
python -m unittest -v
python3 -m unittest -v
~~~
- Sin más argumentos, unittest busca en la carpeta actual los ficheros que empiezan por test y ejecuta sus métodos test_*.
- -v (*verbose*) muestra cada test con ok o FAIL.
- En Linux, si python no existe, usa python3.
- Si algún test falla, el comando termina con error y Actions marca el job en rojo.
- Pruébalo en local antes de subirlo.

## Workflow: tests en tres sistemas con versiones distintas
Cuando el enunciado pide jobs con nombres concretos, cada job va por separado. El nombre del job es la clave que va debajo de jobs:.

~~~yaml
name: Tests de Python

on:
  push:
    branches: [main]

jobs:
  pruebas-linux:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.13'
      - run: python -m unittest -v

  pruebas-windows:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.11'
      - run: python -m unittest -v

  pruebas-mac:
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.12'
      - run: python -m unittest -v
~~~
- Cada job empieza en una máquina limpia: siempre necesita checkout (descargar el código) y setup-python (instalar la versión de Python).
- Los tres jobs se ejecutan en paralelo.
- Cambia los nombres de job, las versiones de Python y las versiones de las acciones (@v4, @v5, @v6…) por las que pida el enunciado.

## La misma idea con una matriz
~~~yaml
jobs:
  pruebas:
    strategy:
      matrix:
        include:
          - os: ubuntu-latest
            python: '3.13'
          - os: windows-latest
            python: '3.11'
          - os: macos-latest
            python: '3.12'
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python }}
      - run: python -m unittest -v
~~~
- La matriz crea **un solo job** con tres variantes («pruebas (ubuntu-latest, 3.13)»…).
- Si el enunciado exige tres jobs con nombres distintos, usa la versión anterior.

## Dónde va el fichero
- Siempre en .github/workflows/nombre.yml.
- Desde la web: **Add file** → **Create new file** y escribe la ruta completa en el nombre; GitHub crea las carpetas al escribir cada /.
- Al hacer commit en main desde la web también hay push, así que el workflow se ejecuta.

## Trampas del YAML
- Sangría de 2 espacios, nunca tabuladores.
- with: va dentro del paso, con más sangría que uses:.
- La versión de Python va **entre comillas**: sin ellas, 3.10 se lee como el número 3.1.
- El nombre de la rama tiene que coincidir (main no es master).

## Ver el resultado
Pestaña **Actions** → el workflow → cada job → despliega el paso de los tests. Verde: todo bien. Rojo: abre el paso y lee el error. Para repetir: **Re-run jobs**.

## Errores típicos
| Síntoma | Causa | Solución |
|---|---|---|
| El workflow no aparece en Actions | El fichero no está en .github/workflows/ o no termina en .yml | Muévelo a la ruta correcta |
| Invalid workflow file | Sangría o clave mal escrita | Revisa espacios y los dos puntos |
| Ran 0 tests | El fichero de tests no empieza por test o está en otra carpeta | Renómbralo o ejecuta desde la carpeta correcta |
| ModuleNotFoundError | Se ejecuta desde otra carpeta y no encuentra el programa | Deja tests y programa en la raíz, o entra en la carpeta |
| Solo falla en un sistema | Diferencias de versión de Python o de rutas | Lee el error de ese job concreto |

## Comprueba que sabes
- [ ] Escribir un test con unittest y ejecutarlo con python -m unittest -v.
- [ ] Crear un workflow que se active con push a main.
- [ ] Hacer tres jobs con runs-on distinto y una versión de Python distinta en cada uno.
- [ ] Explicar la diferencia entre tres jobs y una matriz.
- [ ] Encontrar en la pestaña Actions por qué ha fallado un job.
