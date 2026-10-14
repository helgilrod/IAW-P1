# Práctica 15. Pipeline del blog Next.js

## Objetivo
Automatizar linter, Cypress, artefacto de resultado, badge del README, despliegue Vercel y notificación por correo.

## Paso 1. Preparar proyecto
Parte del fork/copia de antoni-gimenez/nodejs-blog-practica que indique el profesor. Crea la rama de la práctica y sigue el README del proyecto.

## Paso 2. Organizar jobs
- Linter_job: instala dependencias y ejecuta el linter del package.json.
- Cypress_job: ejecuta pruebas y guarda result.txt incluso si fallan.
- Add_badge_job: descarga result.txt, invoca la acción personalizada y actualiza README.
- Deploy_job: despliega con la acción Vercel indicada.
- Notification_job: se ejecuta siempre y envía correo con los resultados.

## Paso 3. Linter
Usa checkout, instala dependencias y ejecuta el script real del package.json. No inventes el nombre del script.

## Paso 4. Guardar resultado de Cypress
El paso de resultado y subida debe ejecutarse aunque las pruebas fallen. Patrón orientativo:
~~~yaml
- name: Guardar resultado
  if: always()
  run: echo "$RESULTADO_TESTS" > result.txt

- name: Subir resultado
  if: always()
  uses: actions/upload-artifact@v4
  with:
    name: cypress-result
    path: result.txt
~~~
Configura RESULTADO_TESTS para capturar el resultado real del paso Cypress según la estructura del workflow; no lo dejes como valor indefinido.

## Paso 5. Actualizar badge
El job de badge debe depender del resultado de pruebas, ejecutarse incluso cuando fallen si así lo pide el enunciado, descargar el artefacto y leer result.txt. Después invoca la acción personalizada que actualiza el README. Para hacer push automático, configura permisos contents: write, identidad Git y commit.

## Paso 6. Desplegar en Vercel
La actividad menciona amondnet/vercel-action@v20. Usa los inputs que indique el enunciado y configura los secretos requeridos en Settings → Secrets and variables → Actions. Nunca escribas tokens en YAML.

## Paso 7. Notificar por correo
Notification_job debe usar una condición de ejecución siempre. Construye el mensaje con los resultados de los jobs y configura credenciales de correo como secretos.

## Paso 8. Documentar y comprobar
Añade al README pasos y capturas solicitados. En Actions revisa linter, Cypress, artefacto, badge, deploy y notificación por separado.

Nota: las entradas exactas de Cypress, Vercel, correo y la acción personalizada dependen de los archivos/versions entregados por el profesor. Respeta ese contrato exacto.