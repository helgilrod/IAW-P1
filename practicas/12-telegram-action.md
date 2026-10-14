# Práctica 12. Acción personalizada para Telegram

## Objetivo
Crear una acción JavaScript que envíe un mensaje a un chat de Telegram al ejecutar el workflow.

## Paso 1. Crear bot y secretos
Crea un bot mediante BotFather, obtén el token y averigua el chat ID. En GitHub configura los secretos TELEGRAM_BOT_TOKEN y TELEGRAM_CHAT_ID. No guardes sus valores en el repo.

## Paso 2. Estructura
~~~text
.github/workflows/send_telegram_workflow.yml
telegram-action/action.yml
telegram-action/package.json
telegram-action/index.js
~~~

## Paso 3. action.yml
~~~yaml
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
~~~

## Paso 4. package.json
~~~json
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
~~~

## Paso 5. index.js
~~~javascript
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
~~~

## Paso 6. Workflow
~~~yaml
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
~~~

## Paso 7. Commit y comprobación
~~~bash
git add telegram-action .github/workflows/send_telegram_workflow.yml
git commit -m "Añadir acción Telegram"
git push
~~~
Comprueba el log y el chat. Para una acción JavaScript distribuida, las dependencias deben estar empaquetadas o instaladas de forma compatible con el runtime; sigue el método de empaquetado de clase.