import telebot
import requests

# Replace with your Telegram bot token and Gemini API key
BOT_TOKEN = "YOUR_BOT_TOKEN"
API_KEY = "YOUR_GEMINI_API_KEY"

bot = telebot.TeleBot(BOT_TOKEN)

@bot.message_handler(commands=["start"])
def start(message):
    bot.send_message(message.chat.id, "Welcome! Send me a message to get creative text from Gemini AI.")

@bot.message_handler(func=lambda message: True)
def handle_message(message):
    text = message.text
    url = f"https://ai.google.dev/v1/textGeneration:generateText?key={API_KEY}"
    headers = {"Content-Type": "application/json"}
    data = {"input": text}
    response = requests.post(url, headers=headers, json=data)
    response.raise_for_status()
    generated_text = response.json()["generatedText"]
    bot.send_message(message.chat.id, generated_text)

bot.polling()
