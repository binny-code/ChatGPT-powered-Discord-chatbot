# 🤖 ChatGPT-Powered Discord Bot (Google Colab)

A fully functional **Discord chatbot** that runs in **Google Colab** and uses **OpenAI’s GPT models** for real-time, natural language responses.  
No local setup or hosting required — just run the notebook, connect your Discord bot token and OpenAI API key, and start chatting.

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/yourusername/discord-chatgpt-bot/blob/main/discord_chatgpt_bot.ipynb)

---

## 🚀 Features
- **Cloud-based Execution** – Run entirely in Google Colab, no server needed.  
- **Real-time Discord Interaction** – Responds instantly to text commands.  
- **AI-Powered Conversations** – Uses OpenAI’s GPT models for intelligent replies.  
- **Custom Commands** – Adjustable prefix and command triggers.  

---

## 📦 Requirements
- **Discord Bot Token** → Get from the [Discord Developer Portal](https://discord.com/developers/applications)  
- **OpenAI API Key** → Get from the [OpenAI Platform](https://platform.openai.com/api-keys)  
- **Google Colab** account  

---

## 📜 How It Works
1. **Create a Discord Bot**
   - Go to the [Discord Developer Portal](https://discord.com/developers/applications)
   - Create a new application, add a bot, and enable "Message Content Intent"
   - Copy your **Bot Token**

2. **Get Your OpenAI API Key**
   - Sign in to [OpenAI Platform](https://platform.openai.com/api-keys)
   - Create a new API key and copy it

3. **Run in Google Colab**
   - Click the **Open in Colab** badge above
   - Paste your **Discord Bot Token** and **OpenAI API Key** into the code
   - Run all cells — your bot will log in and be ready

4. **Chat with the Bot in Discord**
   - In your Discord server, try:
     ```
     !hello
     !tell me a joke
     !write a haiku about data science
     ```

---

## 🛠️ Code Example

```python
!pip install discord.py==2.3.2 nest_asyncio openai

import discord
import nest_asyncio
import asyncio
import openai

nest_asyncio.apply()

DISCORD_TOKEN = "YOUR_DISCORD_BOT_TOKEN"
OPENAI_API_KEY = "YOUR_OPENAI_API_KEY"
PREFIX = "!"

openai.api_key = OPENAI_API_KEY

intents = discord.Intents.default()
intents.message_content = True
client = discord.Client(intents=intents)

async def ask_chatgpt(prompt):
    response = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": prompt}],
        max_tokens=500,
        temperature=0.7
    )
    return response.choices[0].message["content"]

@client.event
async def on_ready():
    print(f"✅ Logged in as {client.user}")

@client.event
async def on_message(message):
    if message.author == client.user:
        return
    if message.content.startswith(PREFIX):
        prompt = message.content[len(PREFIX):].strip()
        if prompt:
            await message.channel.send("💭 Thinking...")
            reply = await ask_chatgpt(prompt)
            await message.channel.send(reply)

async def start_bot():
    await client.start(DISCORD_TOKEN)

try:
    asyncio.get_event_loop().run_until_complete(start_bot())
except KeyboardInterrupt:
    pass

