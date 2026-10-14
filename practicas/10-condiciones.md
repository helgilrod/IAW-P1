# Práctica 10. Paso condicional si fallan las pruebas

## Paso 1. Identificar el paso de pruebas
~~~yaml
- name: Ejecutar pruebas
  id: test
  run: npm test
~~~

## Paso 2. Añadir un paso de aviso
~~~yaml
- name: Avisar de fallo
  if: failure()
  run: echo "Las pruebas han fallado"
~~~

## Ejemplo de job completo
~~~yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v3
        with:
          node-version: 20
      - run: npm install
      - name: Ejecutar pruebas
        id: test
        run: npm test
      - name: Avisar de fallo
        if: failure()
        run: echo "Las pruebas han fallado"
~~~

## Paso 3. Diferenciar condiciones
- success(): pasos anteriores relevantes correctos.
- failure(): hubo un fallo previo relevante.
- always(): ejecutar aunque haya fallos anteriores.

Si el enunciado exige que un job completo se ejecute aunque falle una dependencia, configura needs y una condición apropiada, como if: always().

## Paso 4. Probar
Ejecuta pruebas correctas; después provoca un fallo controlado en una rama de práctica y comprueba que se ejecuta el aviso. Revierte el fallo antes de entregar.