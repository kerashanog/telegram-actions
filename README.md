### Description

Send message base on **GitHub Action** to Telegram user/group/channel using **Telegram Bot**


### Usage

#### 1. Create your own Telegram bot
Create your bot by using [@BotFather](https://telegram.me/BotFather) and follow this official instroduction from Telegram [Creating a new bot](https://core.telegram.org/bots#creating-a-new-bot)
#### 2. Connect your Telegram Bot with your repository on GitHub
* Remember ur bot token
* In ur repo, goto: `Settings` **>** `Secrets` **>** `Actions` and create `New repository secret` with name is what ever u want (ex: `telegram_bot`). Do the same thing with User/Group/Channel id you want bot push msg (ex: `telegram_to`).
#### 3. Use in GitHub Action
Example:
```yml
jobs:
  # This workflow contains a single job called "notify"
  notify:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest
    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      - uses: actions/checkout@v3     
      # Get tagged telegram user from map_users. If you don't want it, mark comments all of this block below or delete it.
      # Will return output name is "reviewer", and value is telegram user of "user" variable. Ex: User = 'youtube' will return reviewer = '@vid'
      # You can access this output by "steps.tagged.outputs.reviewer"
      - name: 'Get tagged telegram user'
        uses: kerashanog/telegram-actions@main
        id: tagged
        with:
          # Map of github-user:telegram-user
          map_users: >
            { "kerashanog": "@kerashanog",
              "gh-user": "@tg-user",
              "youtube": "@vid"
            }
          # Github user want to tag in telegram 
          user: 'youtube'
      # This step will send mesage to Telegram with message is template in "message"
      - name: 'Send message to telegram'
        uses: kerashanog/telegram-actions@main
        id: send-message
        with:
          to: ${{ secrets.TELEGRAM_TO }}
          token: ${{ secrets.TELEGRAM_BOT }}
          format: markdown # format could be markdown or html
          message: |
            🎉 **`${{ github.actor }}`** aka ${{ steps.tagged.outputs.reviewer }} send message from 🍻  
            Repository: https://github.com/${{ github.repository }}     
```
#### Input variables 
* secrets.TELEGRAM_BOT - is token of your Telegram bot just saved in repository secrets with name `TELEGRAM_BOT`
* secrets.TELEGRAM_TO - is user/group/channel id you want bot send message to, just saved this ID in repository secrets with name `TELEGRAM_TO`
* format - Format of message in `markdown` or `html`. See [MarkdownV2 style](https://core.telegram.org/bots/api#markdownv2-style) 
* message - Your predefine message
