# Albany-Hacks
This is my submission for the Albany Hacks Hackathon. It is a discord bot that will transfer posts from Instagram to Discord.

import discord
from discord.ext import commands

TOKEN = open('token.txt', 'r').readline()

# testing space


client = commands.Bot(command_prefix = '.')

# answers with the milisecond latency
@client.command()
async def ping(ctx):
    await ctx.send(f'{round (client.latency * 1000)}ms ')

# add account to list
acntList = list()
@client.command()
async def add(ctx,*,name):
    acntList.append(name.lower())
    print(acntList)
    await ctx.send(name+ ' added')

# remove account from list
@client.command()
async def remove(ctx,*,name):
    for x in range(len(acntList)):
        if acntList[x] == name.lower():
            acntList.remove(acntList[x])
            break
    print(acntList)
    await ctx.send(name+ ' removed')

# show list of accounts
@client.command()
async def list(ctx):
    print(acntList)
    if len(acntList) > 0:
        await ctx.send('Accounts being followed: \n')
        for x in range(len(acntList)):
            await ctx.send(acntList[x])
    else:
        await ctx.send('no accounts currently being followed')

    
#If there is an error, it will answer with an error
@client.event
async def on_command_error(ctx, error):
    await ctx.send(f'Error. Try .help ({error})')

# delete default help command
client.remove_command('help')
# Embeded help with list and details of commands
@client.command(pass_context=True)
async def help(ctx):
    embed = discord.Embed(
        colour = discord.Colour.green())
    embed.set_author(name='Help : list of commands available')
    embed.add_field(name='.ping', value='Returns bot respond time in milliseconds', inline=False)
    embed.add_field(name='.add', value='Adds an account to follow', inline=False)
    embed.add_field(name='.remove', value='Removes an account from following list', inline=False)
    embed.add_field(name='.list', value='see list of followed accounts', inline=False)
    await ctx.send(embed=embed)


client.run(TOKEN)
