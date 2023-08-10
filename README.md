# Telegram Bot on Cloudflare Workers

A minimal example of a Telegram Bot running on a Cloudflare Worker.

## Setup:

1. Get your new bot token from [@BotFather](https://t.me/botfather): https://core.telegram.org/bots#6-botfather
2. Sign up to Cloudflare Workers: https://workers.cloudflare.com/
3. In the Cloudflare Dashboard go to "Workers", then click "Create application" and then "Create worker"
4. Choose a name and click "Deploy" to create the worker
5. Click on "Configure worker" -> "Settings" -> "Variables"
6. Add a new variable with the name `ENV_BOT_TOKEN` and the value of your bot token from [@BotFather](https://t.me/botfather)
7. Add a new variable with the name `ENV_BOT_SECRET` and set the value to a random secret. See https://core.telegram.org/bots/api#setwebhook
8. Click on "Quick Edit" to change the source code of your new worker
9. Copy and paste the code from [bot.js](bot.js) into the editor
10. Optional: Change the `WEBHOOK` variable to a different path. See https://core.telegram.org/bots/api#setwebhook
11. Click on "Save and Deploy"
12. In the middle panel append `/registerWebhook` to the url. For example: https://my-worker-123.username.workers.dev/registerWebhook
13. Click "Send". In the right panel should appear `Ok`. If 401 Unauthorized appears, you may have used a wrong bot token.
14. That's it, now you can send a text message to your Telegram bot

## Bot behaviour

The bot will send the original message back with `Echo:` prepended.
If you want to change it, look at the function `onMessage()`. It receives a [Message](https://core.telegram.org/bots/api#message) object and sends a text back:

```javascript
/**
 * Handle incoming Message
 * https://core.telegram.org/bots/api#message
 */
function onMessage (message) {
  return sendPlainText(message.chat.id, 'Echo:\n' + message.text)
}
```

## bot2.js

The file [bot2.js](bot2.js) contains an improved bot, that demonstrates how to react to commands,
send and receive [inline buttons](https://core.telegram.org/bots/api#inlinekeyboardbutton),
and create [MarkdownV2](https://core.telegram.org/bots/api#markdownv2-style)-formatted text.

## bot3.js

The file [bot3.js](bot3.js) contains an improved version that replies inline queries with voice messages.
The voice messages should be stored in OPUS format and .ogg in the cloud you most like.
The audio files are listed in a JSON array with the following structure in a KV namespace called `NAMESPACE` and with following content under the key `input_files`.

Go to *Workers & Pages* -> *KV* and create a new namespace. Add a new key `input_files` and store the JSON structure from below with your own audio files.

Now in *Overview* -> your-worker -> *Settings* -> *Variables* -> *KV Namespace Bindings* bind the namespace to a variable called `NAMESPACE`.

```javascript
 [
    [
      "File Name",
      "URL",
      duration,
      "<tg-spoiler> caption </tg-spoiler>"
    ],
    [
      "test",
      "https://example.com/my_file.ogg",
      5,
      "<tg-spoiler>Description in a spoiler</tg-spoiler>"
    ],
  ]
```

---

[![JavaScript Style Guide](https://cdn.rawgit.com/standard/standard/master/badge.svg)](https://github.com/standard/standard)
