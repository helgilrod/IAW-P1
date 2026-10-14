# Práctica 15. Automatizar un blog Next.js con linter, Cypress, badge, Vercel y correo

## Objetivo
Construir una pipeline de varios jobs: validar el código, probar la aplicación, conservar resultados, actualizar el README, desplegar y notificar el resultado.

## 1. Preparar el proyecto
Parte del fork o copia de `antoni-gimenez/nodejs-blog-practica` que indique el profesor. Lee su README y comprueba `package.json`: los scripts reales pueden llamarse `lint`, `test`, `cypress` u otra cosa. Crea la rama indicada por el enunciado.

## 2. Entender los jobs y dependencias
- **Linter_job:** instala dependencias y ejecuta el linter.
- **Cypress_job:** ejecuta pruebas de navegador y guarda un resultado.
- **Add_badge_job:** descarga el resultado y actualiza el README con la acción personalizada.
- **Deploy_job:** publica el proyecto en Vercel.
- **Notification_job:** envía un correo con los resultados.

Usa `needs` para indicar qué jobs deben terminar antes de otros. Cada job tiene un runner independiente; los archivos se transfieren con artefactos, no aparecen mágicamente en todos los jobs.

## 3. Configurar el linter
Usa `actions/checkout`, instala dependencias y ejecuta el script que realmente exista en `package.json`. No inventes el nombre del script. Revisa el log del job si el comando no existe o el análisis devuelve errores.

## 4. Guardar el resultado de Cypress
La actividad pide conservar el resultado incluso si falla la prueba. Este patrón es orientativo:
```yaml
- name: Guardar resultado
  if: always()
  run: echo "$RESULTADO_TESTS" > result.txt

- name: Subir resultado
  if: always()
  uses: actions/upload-artifact@v4
  with:
    name: cypress-result
    path: result.txt
```
El fragmento no captura por sí solo el resultado real. Debes asignar `RESULTADO_TESTS` a partir del estado del paso Cypress o utilizar el contexto/outputs correspondiente. Si la variable queda sin definir, el archivo no describirá el resultado.

## 5. Actualizar el badge
El job de badge descarga el artefacto, lee `result.txt` e invoca la acción personalizada. Si debe subir el README modificado, configura el permiso mínimo necesario (`contents: write`) y los pasos de identidad Git, commit y push. Comprueba que solo cambia la zona prevista.

## 6. Desplegar en Vercel
La actividad menciona `amondnet/vercel-action@v20`. Usa los inputs y secretos exactos del enunciado y de la versión indicada. Guarda credenciales en **Settings → Secrets and variables → Actions**, nunca como texto en el YAML.

## 7. Enviar notificación por correo
La notificación puede tener que ejecutarse incluso si un job anterior falla. Configura la condición indicada por el profesor y construye el mensaje con los resultados reales de la pipeline. Guarda credenciales del servicio de correo como secretos.

## 8. Ejecutar y documentar
En Actions revisa cada job por separado: linter, Cypress, artefacto, badge, deploy y correo. Comprueba también qué jobs se ejecutan cuando falla Cypress. Añade las capturas que solicite la práctica.

**Importante:** los inputs de Cypress, Vercel, correo y la acción personalizada dependen del proyecto y del material del profesor. Coteja esos parámetros con los archivos originales antes de entregar; esta guía explica el flujo sin inventar contratos de acciones.