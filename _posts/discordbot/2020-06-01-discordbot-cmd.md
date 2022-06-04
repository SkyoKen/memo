---
layout: 	post
title: 		'【discordbot】添加指令功能'
date:       	2020-05-31 18:00:00
header-img: 	"/img/post/post-discord.png"
tags:
  - discordbot
---
>接上篇《【discordbot】写个复读机bot》


## 加个开始运行时频道内提示功能与复读功能
添加指令
```python
import discord
from discord.ext import commands

TOKEN = '' #去discord查自己新建的bot的TOKEN是多少

prefix = '$' 

# 写个中文指令
class Test(commands.Cog, name ='测试'):
    def __init__(self,bot):
        super().__init__()
        self.bot = bot

    @commands.command()
    async def repeat(self,ctx,arg):
        """复述字符串"""
        await ctx.send(arg)

    @commands.command(name="猫图")
    async def cat(self,ctx):
        """输出猫图"""
        await ctx.send("https://media.giphy.com/media/JIX9t2j0ZTN9S/giphy.gif")

    @commands.command(name="汪")
    async def wang(self,ctx):
        """输出三个汪的字符串"""
        await ctx.send("汪汪汪")

# 自己写个中文指令说明替换默认的
class ChineseHelpCommand(commands.DefaultHelpCommand):
    def __init__(self):
        super().__init__()
        self.commands_heading = "命令："
        self.no_category = "其他"
        self.command_attrs["help"] = "命令一栏与简单说明"

    def get_ending_note(self):
        return ("测试用")

bot = commands.Bot(command_prefix = prefix, help_command = ChineseHelpCommand())
bot.add_cog(Test(bot = bot))
# 启动
bot.run(TOKEN)
```

## 执行代码后去频道测试结果

指令使用正常
![](/img/post/in-post/post-discord-20053104.png)
![](/img/post/in-post/post-discord-20053105.png)