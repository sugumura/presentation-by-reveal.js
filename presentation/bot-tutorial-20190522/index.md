# オトナのPython入門#Discord_Bot編
2019年5月22日

オトナのプログラミング勉強会<br>
協力 **未来会議室**

[connpass](https://otona.connpass.com)<!--- .element target="_blank" rel="noopener" -->

---

# 自己紹介

- 村上　卓
- フリーランス
- Angular/Ruby On Rails

---

オトナのプログラミング勉強会<br>
[http://otona.pro](http://otona.pro)<!--- .element target="_blank" rel="noopener" -->

- 2016年8月から開始
- 月2回（第1水曜、第3水曜）
- いつでも講師募集中
    - プログラム言語、機械学習、Web系...
- [YouTube Live](https://www.youtube.com/channel/UCrXf76sF5RUKcGpMpZASqow)<!--- .element target="_blank" rel="noopener" -->でリモート参加も振り返りも可(?)

---

# Discord Botを作ろう

---

# Discord

- ゲーマー向けボイス＆テキストチャット
- 自分でサーバを立ててチャンネルを作成
- Steamなどのゲームプラットフォームの連携がすごい
- SlackとかSkypeとかMSN(Live)メッセンジャーみたいなの

---

# BOT

- 人間に変わって作業をしてくれるロボット
- DiscordではBOTユーザとして参加
- BOT機能を自分でつくろう
  - 挨拶
  - おみくじ
  - 簡易電卓

---

# 前準備

自分のサーバを作成しよう

- Discordアカウントを作成
- サーバを追加


---

# 前準備#1

![pre01](../../image/20190522/pre01.png)

---

# 前準備#2

![pre02](../../image/20190522/pre02.png)

---

# 前準備#3

任意のサーバ名を入力

![pre03](../../image/20190522/pre03.png)

---

# 開発環境#1

- Python 3.5.3以降

Python実行環境がない方は以下のURLにアクセス  
[https://coder.otona.pro](https://coder.otona.pro)<!--- .element target="_blank" rel="noopener" -->


---

# 開発環境#2

discord.pyをpipでダウンロード(coderの方は必要なし)

```console
# Mac or Linux
$ python3 -m pip install -U discord.py

# Windows
$ py -3 -m pip install -U discord.py
```

[https://discordpy.readthedocs.io/ja/latest/](https://discordpy.readthedocs.io/ja/latest/)<!--- .element target="_blank" rel="noopener" -->

---

# ワークスペース作成(coder)

他の方とファイルが被らないように自分のフォルダを作成  
[Terminal] -> [New Terminal]

```console
$ mkdir [YOUR_ID]
$ cd [YOUR_ID]
```

---

# メインファイル作成

```
$ touch bot.py
```

---

# 雛形

```python
# bot.py
import discord
# あとで取得したものを↓と差し替えてください
TOKEN = 'YOUR_ACCESS_TOKEN'

client = discord.Client()

@client.event
async def on_ready():
    print("ready call!")

@client.event
async def on_message(message):
    if message.author.bot:
        return
    if message.content == '/neko':
        await message.channel.send('にゃー')

client.run(TOKEN)
```

---

# アクセストークントークン#1

以下のURLにアクセス

[https://discordapp.com/developers/applications/](https://discordapp.com/developers/applications/)<!--- .element target="_blank" rel="noopener" -->

---

# アクセストークントークン#2
![01](../../image/20190522/01.png)

---

# アクセストークントークン#3
![02](../../image/20190522/02.png)

---

# アクセストークントークン#4
![03](../../image/20190522/03.png)

---

# アクセストークントークン#5
![04](../../image/20190522/04.png)

---

# アクセストークントークン#6
![05](../../image/20190522/05.png)

---

# アクセストークントークン#7
![06](../../image/20190522/06.png)

---

# アクセストークントークン#8
![07](../../image/20190522/07.png)

---

# アクセストークントークン#9
![08](../../image/20190522/08.png)

---

# アクセストークントークン#10
![09](../../image/20190522/09.png)

---

# BOT実行

```console
$ cd [YOUR_ID]
$ python3 bot.py 
ready call!

# アクション待ち
# Ctrl+cで終了
```

---

# テスト

![test01](../../image/20190522/test01.png)

---

# BOTができた！

---

# 構文解説

`@client.event`とか`async/await`って何？

---

# デコレータ#1

`@client.event`のように@から始まる装飾を`デコレータ`と呼ぶ
Pythonでは以下の種類をサポート

- 関数
- メソッド
- クラス

---

# デコレータ#2

デコレータはシンタックスシュガー  
以下の2つのコードはほぼ同じ意味

```
@f1(arg)
@f2
def func(): pass
```

```
def func(): pass # 違いはここでfuncが一度定義される
func = f1(arg)(f2(func))
```

参考: [Python 言語リファレンス](https://docs.python.org/ja/3/reference/compound_stmts.html#function)<!--- .element target="_blank" rel="noopener" -->

---

# デコレータ#3

実行サンプル

```python
# deco.py
def deco(func):
    def wrapper(*args, **kwargs):
        print('---start---')
        func(*args, **kwargs)
        print("---end---")
    return wrapper

@deco
def main():
    print('Hello Decorator')

if __name__ == "__main__":
    main()
```

参考: [Pythonのデコレータについて - Qiita](https://qiita.com/mtb_beta/items/d257519b018b8cd0cc2e)<!--- .element target="_blank" rel="noopener" -->

---

# デコレータ#4

```console
$ python3 deco.py
---start---
Hello Decorator
---end---
```

---

# デコレータ#5

discord.pyのソースを確認  
@client.eventで渡した関数をdiscord.pyでプロパティに追加  

```python
class Client:
    # 省略
    # コメント省略
    def event(self, coro):
        if not asyncio.iscoroutinefunction(coro):
            raise TypeError('event registered must be a coroutine function')
        # setattrでプロパティに追加される
        setattr(self, coro.__name__, coro)
        log.debug('%s has successfully been registered as an event', coro.__name__)
        return coro
```


参考: [https://github.com/Rapptz/discord.py/blob/master/discord/client.py](https://github.com/Rapptz/discord.py/blob/master/discord/client.py)<!--- .element target="_blank" rel="noopener" -->


---

# async/await #1

コルーチンを扱うための構文  
ここではソースを読むための表層的な部分のみ紹介する  

---

# async/await #2

asyncと一緒に宣言をした関数はコルーチンになる  
通常の関数呼び出しではエラー

```python
import asyncio

async def hello_world():
    print("Hello World!")

# イベントループを取得
loop = asyncio.get_event_loop()
# イベントループに呼び出しを任せる
loop.run_until_complete(hello_world())
loop.close()
```

参考: [python3 の async/awaitを理解する - Qiita](https://qiita.com/maueki/items/8f1e190681682ea11c98)<!--- .element target="_blank" rel="noopener" -->

---

# async/await #3

awaitを利用することでasync関数を呼び出せる

```python
import asyncio

async def hello_world():
    print("Hello World!")

async def call_hello_world():
    await hello_world()

loop = asyncio.get_event_loop()
loop.run_until_complete(call_hello_world())
```

---

# async/await #4

もう一度discord.pyのソースを確認  
コルーチンを受け取ることを期待していることがわかる

```python
class Client:
    # 省略
    # コメント省略
    def event(self, coro):
        if not asyncio.iscoroutinefunction(coro):
            raise TypeError('event registered must be a coroutine function')
        # setattrでプロパティに追加される
        setattr(self, coro.__name__, coro)
        log.debug('%s has successfully been registered as an event', coro.__name__)
        return coro
```

参考: [https://github.com/Rapptz/discord.py/blob/master/discord/client.py](https://github.com/Rapptz/discord.py/blob/master/discord/client.py)<!--- .element target="_blank" rel="noopener" -->

---

# 休憩（質疑応答）

---

# おみくじを作ろう#1

簡単なおみくじ機能を実装

- Random機能を使って運勢を表示
- DiscordのEmbed表示を使用
- リアクションを使って引き直し可能

---

# おみくじを作ろう#2

最初にシンプルなおみくじを実装してテスト

```python
import random # 読み込み追加

# 省略(on_message内)
    if message.content == '/omikuji':
        value = omikuji(random.randint(0, 100))
        await message.channel.send(value)

def omikuji(num = 0):
    if num < 10:
        return "大吉"
    elif 10 <= num < 30:
        return "中吉"
    elif 30 <= num < 60:
        return "小吉"
    elif 60 <= num < 80:
        return "吉"
    elif 80 <= num < 95:
        return "末吉"
    else:
        return "凶"
```

---

# おみくじを作ろう#3

埋め込み形式にしよう

```python
# 省略(on_message内)
    if message.content == '/omikuji':
        value = omikuji(random.randint(0, 100))
        await send_omikuji(message.channel, message.author, value)

async def send_omikuji(channel, user, value):
    embed = discord.Embed(title="おみくじ",
                        description=f"{user.mention}さんの今日の運勢は！",
                        color=discord.Colour.from_rgb(255,0,0))
    embed.set_thumbnail(url=user.avatar_url)
    embed.add_field(name="運勢: ", value=value, inline=False)
    await channel.send(embed=embed)
```

---

# おみくじを作ろう#4

リアクションを実装しよう

```python
# リアクションがあった場合に呼び出し
@client.event
async def on_reaction_add(reaction, user):
    if (reaction.emoji == '⭕'):
        value = omikuji(random.randint(0, 100))
        await send_omikuji(reaction.message.channel, user, value)

async def send_omikuji(channel, user, value):
    # 省略
    embed.add_field(name="運勢: ", value=value, inline=False)
    embed.add_field(name="もう一度?", value="⭕(:o:)をつけるともう一度くじを引きます", inline=False)
    await channel.send(embed=embed)
```

---

# 休憩（質疑応答）

---

# 簡易電卓#1

- evalを使って簡易電卓
- 公開BOTでこんな実装してはいけません

あくまで機能例としてお願いします🙇

---

# 簡易電卓#2

```python
import re
# 正規表現で引数を取得するための定義
calc_pattern = re.compile('\A\/calc (.+)\Z')

# 省略(on_message内)
    result = calc_pattern.match(message.content)
    if result:
        exp = result.group(1)
        res = eval(exp) # 公開BOTでevalを使わないでください
        await message.channel.send(res)
```

---

# 資料はここまで

---

# 自由時間

思いつくままに触ってみてください

- 他APIと連携(天気予報、カレンダー、Todo...)
- Discordイベントの利用(参加者追加、Botへのメンション...)
- 既存のDiscordやSlackのBotを参考に

---

# BOT公開のために

- BOT権限を最小にする
    - 今回は管理者権限を付与。公開する場合は権限を必要なものだけに
- HerokuやGCPの無料枠を活用しよう

---

# 資料

- [discord.pyへようこそ。](https://discordpy.readthedocs.io/ja/latest/)<!--- .element target="_blank" rel="noopener" -->
- [Pythonで実用Discord Bot(discordpy解説) - Qiita](https://qiita.com/1ntegrale9/items/9d570ef8175cf178468f)<!--- .element target="_blank" rel="noopener" -->
- [Pythonにおける非同期処理: asyncio逆引きリファレンス](https://qiita.com/icoxfog417/items/07cbf5110ca82629aca0)<!--- .element target="_blank" rel="noopener" -->

---

# おつかれさまでした🍵
