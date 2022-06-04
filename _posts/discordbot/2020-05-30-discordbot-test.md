---
layout: 	post
title: 		'【discordbot】尝试做个discordbot'
subtitle: 	"在树莓派下运行discordbot"
date:       	2020-05-31 12:00:00
header-img: 	"/img/post/post-discord.png"
tags:
  - Raspberry Pi
  - discordbot
---
## 初期设定
创建bot
https://qiita.com/1ntegrale9/items/cb285053f2fa5d0cccdf
树莓派准备
```
sudo pip3 install discord.py
```
## 写代码
创建`test.py`文件测试连接
```
import discord

#看初期设定
TOKEN = ''

client = discord.Client()

# 启动时
@client.event
async def on_ready():
    print('login')

# 有消息时
@client.event
async def on_message(message):
    # 无视bot消息
    if message.author.bot:
        return
    # 如果是test，则恢复测试
    if message.content == 'test':
        await message.channel.send('测试')

# bot与服务器连接
client.run(TOKEN)
```

## 测试

![](/img/post/in-post/post-discord-20053101.png)