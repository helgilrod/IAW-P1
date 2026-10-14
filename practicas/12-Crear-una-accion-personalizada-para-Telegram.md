# Práctica 12. Crear una acción personalizada que envía mensajes a Telegram

## Objetivo
Construir una acción JavaScript local y reutilizable para enviar una notificación a un chat de Telegram.

## 1. Preparar bot y secretos
Crea un bot con BotFather, consigue el token y averigua el ID del chat de destino. En GitHub abre **Settings → Secrets and variables → Actions** y crea `TELEGRAM_BOT_TOKEN` y `TELEGRAM_CHAT_ID`. No publiques los valores ni los incluyas en el código.

## 2. Crear la estructura
```text
.github/workflows/send_telegram_workflow.yml
telegram-action/action.yml
telegram-action/package.json
telegram-action/index.js
```
La carpeta `telegram-action` contiene el código de la acción; el workflow la invocará con `uses: ./telegram-action`.

## 3. Definir action.yml
```yaml
name: Enviar mensaje a Telegram
description: Envía una notificación a Telegram
inputs:
  bot-token:
    description: Token del bot
    required: true
  chat-id:
    description: Chat de destino
    required: true
runs:
  using: node20
  main: index.js
```
Este descriptor declara los inputs que debe recibir la acción y el archivo JavaScript que ejecutará GitHub.

## 4. Declarar dependencias en package.json
```json
{
  "name": "telegram-notification-action",
  "version": "1.0.0",
  "private": true,
  "main": "index.js",
  "dependencies": {
    "@actions/core": "^1.10.1",
    "node-telegram-bot-api": "^0.66.0"
  }
}
```
`@actions/core` permite leer inputs y marcar errores; `node-telegram-bot-api` permite enviar el mensaje.

## 5. Crear index.js
```javascript
const core = require('@actions/core');
const TelegramBot = require('node-telegram-bot-api');

async function main() {
  try {
    const token = core.getInput('bot-token', { required: true });
    const chatId = core.getInput('chat-id', { required: true });
    const bot = new TelegramBot(token, { polling: false });
    const message = "Workflow executat correctament després de l'últim commit. Salutacions NOM";
    await bot.sendMessage(chatId, message);
    core.info('Missatge enviat');
  } catch (error) {
    core.setFailed(error.message);
  }
}
main();
```
El código obtiene las credenciales desde inputs, envía el texto y comunica los errores a GitHub Actions.

## 6. Invocar la acción
Crea `.github/workflows/send_telegram_workflow.yml`:
```yaml
name: Enviar Telegram
on:
  push:
    branches: [send_telegram_action]
  workflow_dispatch:
jobs:
  envio_missatge:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Instalar dependencias
        run: cd telegram-action && npm install
      - name: Enviar mensaje
        uses: ./telegram-action
        with:
          bot-token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
          chat-id: ${{ secrets.TELEGRAM_CHAT_ID }}
```

## 7. Probar y diagnosticar
Sube los archivos y revisa el log y el chat. Si falla, verifica token, chat ID, permisos del bot y carga de dependencias. Las acciones JavaScript deben poder resolver sus dependencias durante la ejecución: sigue el método de empaquetado que haya explicado el profesor; instalar paquetes en un paso no siempre basta para una acción local que se ejecuta en otro contexto.