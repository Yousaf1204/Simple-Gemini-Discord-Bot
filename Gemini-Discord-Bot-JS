const discord = require("discord.js");
const { GoogleGenerativeAI } = require("@google/generative-ai");
const MODEL = "gemini-pro";
const API_KEY = process.env.API_KEY ?? "";
const BOT_TOKEN = process.env.BOT_TOKEN ?? "";
const CHANNEL_ID = process.env.CHANNEL_ID ?? "";

const ai = new GoogleGenerativeAI(API_KEY);
const model = ai.getGenerativeModel({
  model: MODEL,
  generationConfig: { maxOutputTokens: 2000 },
});

const { ActivityType } = require("discord.js");

const client = new discord.Client({
  intents: [
    discord.GatewayIntentBits.Guilds,
    discord.GatewayIntentBits.GuildMessages,
    discord.GatewayIntentBits.MessageContent,
    discord.GatewayIntentBits.DirectMessages,
  ],
  partials: ["CHANNEL", "MESSAGE", "REACTION", "USER"],
});

client.on("ready", () => {
  console.log(`Logged in as ${client.user.tag}!`);
  client.user.setActivity("your Requests", {
    type: ActivityType.Listening,
  });
});

client.login(BOT_TOKEN);

client.on("messageCreate", async (message) => {
  try {
    if (message.author.bot) return;

    // Respond to messages in the specific channel
    if (message.channel.id !== CHANNEL_ID) return;

    // Send a placeholder "Typing..." message
    const placeholderMessage = await message.reply(
      "https://tenor.com/view/cat-loading-error-angry-whydoesthishappen-gif-8985245",
    );

    const { response } = await model.generateContent(message.cleanContent);
    const responseText = response.text();

    if (responseText.length > 2000) {
      console.log(
        "Error: Response exceeds the maximum allowed length of 2000 characters.",
      );
      await placeholderMessage.edit(
        "Sorry, The Response is too big which exceeds discord words limit per Message. Try adding modifiers e.g., within 200 words",
      );
      return;
    }

    // Edit the placeholder message with the actual response
    await placeholderMessage.edit({
      content: responseText,
    });
  } catch (e) {
    console.log(e);
  }
});

function logMessage() {
  console.log("Logging message every 10 seconds");
}
setInterval(logMessage, 10000);
