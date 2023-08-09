# Telegram Bot on Cloudflare Workers

A minimal example of a Telegram Bot running on a Cloudflare Worker.

## Setup:

1. Get your new bot token from [@BotFather](https://t.me/botfather): https://core.telegram.org/bots#6-botfather
2. Sign up to Cloudflare Workers: https://workers.cloudflare.com/
3. In the Cloudflare Dashboard go to "Workers" and then click "Create a Service"
4. Choose a name and click "Create a Service" to create the worker
5. Click on "Quick Edit" to change the source code of your new worker
6. Copy and paste the code from [bot.js](bot.js) into the editor
7. Replace the ENV_BOT_TOKEN variable in the code with your token from [@BotFather](https://t.me/botfather) or set ENV_BOT_TOKEN unter settings variables and encrypt it
8. Optional: Change the `WEBHOOK` variable to a different path and the ENV_BOT_SECRET variable to a random secret or  set ENV_BOT_TOKEN unter settings variables and encrypt it. See https://core.telegram.org/bots/api#setwebhook
9. Click on "Save and Deploy"
10. In the middle panel append `/registerWebhook` to the url. For example: https://my-worker-123.username.workers.dev/registerWebhook
11. Click "Send". In the right panel should appear `Ok`. If 401 Unauthorized appears, you may have used a wrong bot token.
12. That's it, now you can send a text message to your Telegram bot

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

The file [bot2.js](bot2.js) contains an improved bot, that demonstrates how to react to commands,
send and receive [inline buttons](https://core.telegram.org/bots/api#inlinekeyboardbutton),
and create [MarkdownV2](https://core.telegram.org/bots/api#markdownv2-style)-formatted text.

The file [bot3.js](bot3.js) contains an improved version of bot1 that replies inline queries with voice messages.
For that the voice messages should be stored in OPUS format and .ogg in the cloud you most like.
The audio files are stored in a JSON array with the follwing structure in a KV namespace called "NAMESPACE" and with this content (filled in) as "input_files".
```json
 [
    [
      "File Name",
      "URL",
      duration,
      "<tg-spoiler> caption </tg-spoiler>"
    ],
    [
      "test",
      "some link",
      5
      "<tg-spoiler>test</tg-spoiler>"
    ]
  ]
```
On Cloudflare workers go to settings -> variables
Set the ENV_BOT_TOKEN and ENV_BOT_SECRET
On Workers & Pages -> KV create a namespace with the json content as input file.
Now in Overview ->your-worker -> Settings -> varaibles-> KV Namespace Bindings bind the NAMESPACE with a variable called NAMESPACE.

---

[![JavaScript Style Guide](https://cdn.rawgit.com/standard/standard/master/badge.svg)](https://github.com/standard/standard)
