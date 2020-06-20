---
layout: 	post
title: 		'【discordbot】写个复读机bot'
date:       	2020-05-31 15:00:00
author: 	"SKyoKen"
header-img: 	"img/post/post-discord.png"
#header-style: 	text
tags:
  - discordbot
---
>接上篇《尝试做个discordbot》


## 加个开始运行时频道内提示功能与复读功能
```python
import discord
import time

TOKEN = ''	#写自己的

client = discord.Client()


# 启动时
@client.event
async def on_ready():
    # 命令行输出
    localtime = time.asctime( time.localtime(time.time()) )
    msg = 'bot启动　' + time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())
    print(msg)

    # 频道内输出
    CHANNEL_ID =  #查查该频道的ID，是一串数字
    channel = client.get_channel(CHANNEL_ID)
    await channel.send(msg)

# 收到消息时
@client.event
async def on_message(message):
    # 无视bot发言
    if message.author.bot:
        return
    # 复读机
    if len(message.content)>2 and message.content[0:2] == '复读':
        await message.channel.send(message.content[2:])



# 启动
client.run(TOKEN)
```

## 执行代码后去频道测试结果

bot成功启动
![](/img/post/in-post/post-discord-20053102.png)

复读功能正常
![](/img/post/in-post/post-discord-20053103.png)