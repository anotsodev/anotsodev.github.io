---
layout: post
title: "Building a Slack Bot with Python"
source: "https://anotsodev.me/building-a-slack-bot-with-python/"
author:
  - "anotsodev"
date: 2017-09-28
excerpt: "The slack bot that I will be building has a feature that will automatically fetch the latest tweets from my subscribed lists (Information Security related) on Twitter. Thanks to this guide I was ab…"
---
The slack bot that I will be building has a feature that will automatically fetch the latest tweets from my subscribed lists (Information Security related) on Twitter.

Thanks to this guide I was able to understand how to build a simple slack bot.

https://www.fullstackpython.com/blog/build-first-slack-bot-python.html

## Requirements

1. Python 2.x.x or 3.x.x
2. Slack Account with a workspace to access the slack API
3. slackclient python library
4. Twitter account for the API access and subscribed with at least 1 list
5. twitter python library

## Getting Started

After installing either Python 2.x.x or 3.x.x, use the following commands to install the needed python libraries.

```
pip install slack client
pip install twitter
```

## Getting the Slack API Access Token

After running the following commands, login to slack and make a workspace for you to be able to generate Slack API Tokens.

Click the site below to create new bot user.

https://api.slack.com/bot-users

Click the “creating a new bot user” to create a new bot user.

Below is the screenshot on creating a new bot user.

![Screenshot-2017-9-28 Bot Users](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/09/screenshot-2017-9-28-bot-users.png?resize=654%2C365&ssl=1)

Enter the username of your bot.

![Screenshot-2017-9-28 Bots](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/09/screenshot-2017-9-28-bots.png?resize=960%2C557&ssl=1)

You will be redirected here after you clicked the Add bot integration button.

![Screenshot-2017-9-28 Bots Slack App Directory](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/09/screenshot-2017-9-28-bots-slack-app-directory.png?resize=970%2C473&ssl=1)

You have now the API token for your bot. Please note the warning message.

You can also customize your bot like uploading a profile picture and describing the features of your bot. After customizing your bot, scroll down and click the Save Integration button.

The most practical method of securing secret tokens it to export it as an environment variable.

```
export SLACK_BOT_TOKEN='your slack token pasted here'
```

But what I will be doing is paste the token to a config.py file and access it using the python file of the bot.

## Getting the Bot’s ID

Here is the modified code snippet from https://www.fullstackpython.com/blog/build-first-slack-bot-python.html

```
import os
from slackclient import SlackClient

config = {}
execfile("config.py", config)

BOT_NAME = 'kairubot'

slack_client = SlackClient(config['slack_api'])

if __name__ == "__main__":
    api_call = slack_client.api_call("users.list")
    if api_call.get('ok'):
    # retrieve all users so we can find our bot
    users = api_call.get('members')
       for user in users:
          if 'name' in user and user.get('name') == BOT_NAME:
              print("Bot ID for '" + user['name'] + "' is " + user.get('id'))
          else:
              print("could not find bot user with the name " + BOT_NAME)
```

Running this program will output the Bot’s ID.

```
C:\Users\Kyle\Documents\Projects\slack-bot>python test.py
Bot ID for 'kairubot' is U79K9K9FB
```

You can now copy and paste the ID of your bot to the config.py file.

Below is the template of the config.py file.

```
# Twitter Access Tokens
consumer_key = ""
consumer_secret = ""
access_key = ""
access_secret = ""

# Slack Access Tokens
slack_api = ""
bot_id = "U772P5C83"
```

## Getting the Twitter Access Tokens

Now we already have our Slack API Token for our bot and the ID of our bot, next step is to get the tokens for user authentication needed to use the Twitter API.

Click the link below to generate these tokens.

https://apps.twitter.com/app/new

You must be logged in on twitter before creating an application.

You just need to fill up the following fields to create your new application.

![Screenshot-2017-9-28 Twitter Application Management](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/09/screenshot-2017-9-28-twitter-application-management.png?resize=1058%2C657&ssl=1)

After filling up the necessary fields and created your new application, you will be redirected to your application’s management page. In order to generate your access tokens, click the Keys and Access Tokens tab then scroll down then click the create new access token button there.

Your application now has generated your access tokens. You just need to paste the tokens on the config.py

Example config.py

```
# Twitter Access Tokens
consumer_key = "k1v..."
consumer_secret = "HsDc..."
access_key = "770..."
access_secret = "5UN..."

#Slack Access Tokens
slack_api = "xoxb-"
bot_id = "U772P5C83"
```

You now have the necessary tokens to make an API requests to Slack and Twitter.

You can now use the source code of the program below to use and retrieve latest tweets from the subscribed lists.

I’ve used the examples here on how to use the Twitter API to get tweets from Twitter.

https://github.com/ideoforms

The feature of this bot is to manually or automatically fetch latest tweets from the subscribed lists.

```
import time
import sys
from slackclient import SlackClient
from twitter import *

config = {}
execfile("config.py", config)

#bot's id 
BOT_ID = config["bot_id"]

# instantiate slack
# Note: always secure the api token
slack_client = SlackClient(config["slack_api"])

AT_BOT = "<@" + BOT_ID + ">"

# list of commands
EXAMPLE_COMMANDS = "help, retrieve, autoretrieve"
COMMANDS = ['help', 'retrieve', 'autoretrieve']

#constants
DEFAULT_INTERVAL = 5

def auto_retrieve(channel, start, default_interval):
    n = default_interval;
    response = "Starting...\n"
    time.sleep(3)
    response += "Will retrieve statuses from the infosec list every 1 hour. Auto Retrieve Intervals = "+str(DEFAULT_INTERVAL)+"\n"
    slack_client.api_call("chat.postMessage", channel=channel,
                                  text=response, as_user=True)
    while start and n > 0:
        response = retrieve()
        slack_client.api_call("chat.postMessage", channel=channel,
                                  text=response, as_user=True)
        n-=1
        # 3600 seconds = 1 hour
        time.sleep(3600)
    return "Auto retrieval stopped."

def retrieve():
    # will use this example on retrieving tweets https://github.com/ideoforms/python-twitter-examples/blob/master/twitter-home-timeline.py
    # config.py https://github.com/ideoforms/python-twitter-examples/blob/master/config.py
    response = ""
    response += "Retrieving 50 tweets... \n\n"

    users = [ "kylehalog" ]

    twitter = Twitter(auth = OAuth(config["access_key"], config["access_secret"], config["consumer_key"], config["consumer_secret"]))

    for user in users:
        result = twitter.lists.list(screen_name = user)
        for list in result:
            list_slug = list["slug"]
            list_str_id = list["id"]
            tweet_count = 50
            statuses = twitter.lists.statuses(slug = list_slug, list_id = list_str_id, count = tweet_count)
            for status in statuses:
                response += "(%s) @%s %s" % (status["created_at"], status["user"]["screen_name"], status["text"]) + "\n"
    return response

def command_handler(command, channel):

    response = "Sorry but I don't get what you mean. I can only understand these commands: " + EXAMPLE_COMMANDS
    if command in COMMANDS:
        if command == 'help':
            response = "\
            List of available commands: \n \
            help - display this message\n \
            retrieve - retrieve and output the latest tweets about infosec\n \
            autoretrieve - automatically retrieve and output tweets about infosec every 1 hour"
        
        elif command == 'retrieve':
            response = retrieve()

        elif command == 'autoretrieve':
            response = auto_retrieve(channel, True, DEFAULT_INTERVAL)
            
                
    slack_client.api_call("chat.postMessage", channel=channel,
                          text=response, as_user=True)
def slack_output_parser(rtm_output):
    
    output_list = rtm_output
    if output_list and len(output_list) > 0:
        for output in output_list:
            if output and 'text' in output and AT_BOT in output['text']:
                # return text after the @ mention, whitespace removed
                return output['text'].split(AT_BOT)[1].strip().lower(), \
                       output['channel']
    return None, None

if __name__ == "__main__":
    READ_WEBSOCKET_DELAY = 1
    if slack_client.rtm_connect():
        print("Kairu Bot is connected and running!")
        while True:
            command, channel = slack_output_parser(slack_client.rtm_read())
            if command and channel:
                command_handler(command, channel)
            time.sleep(READ_WEBSOCKET_DELAY)
    else:
        print("Connection Failed. Invalid Slack token or Bot ID")
```

## Starting the Bot

To run the program, just use this command and invite your bot to your slack channel.

```
python kairu-bot.py
```

Example bot usage on slack

![Screenshot-2017-9-28 supersecretchannel Project Graduate Slack](https://i0.wp.com/anotsodev.me/wp-content/uploads/2017/09/screenshot-2017-9-28-supersecretchannel-project-graduate-slack.png?resize=1127%2C495&ssl=1)

So this is how to build a slack bot with python.

You are free to modify and use the source code on my Github below. You can also add features as many as you want.

https://github.com/anotsodev/slack-bot

Thank you for your time! 🙂

*Featured Image credits to* *[wired.com](https://www.wired.com/wp-content/uploads/2015/08/SlackBot-featured1.jpg)*
