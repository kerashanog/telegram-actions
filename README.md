### Description

Send message base on **GitHub Action** to Telegram user/group/channel using **Telegram Bot**


### How to use

#### 1. Create your own Telegram bot
Create your bot by using [@BotFather](https://telegram.me/BotFather)
#### 2. Connect your Telegram Bot with your repository on GitHub
* Remember ur bot token
* In ur repo, goto: `Settings > Secrets > Actions` and create `new repo secret` with name is what ever u want (ex: `telegram_bot`). Same with User/Group/Channel id you want bot push msg.
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
            # Get tagged telegram user from map_users. If you don't want it, mark comments all of this block
            - name: 'Get tagged telegram user' 
                uses: ./
                id: tagged 
                with: 
                     # Map of github-user:telegram-user 
                     map_users: > 
                                     { "kerashanog": "@kerashanog", 
                                        "gh-user": "@tg-user" } 
                      #user: ${{ github.actor }} 
                      # Github user want to tag in telegram 
                      user: 'gh-user' 
                  # Send mesage to Telegram 
            - name: 'Send message to telegram' 
                uses: ./ 
                id: send-message 
                with: 
                      to: ${{ secrets.TELEGRAM_TO }} 
                      token: ${{ secrets.TELEGRAM_BOT }} 
                      format: markdown 
                      message: | 
                                      🎉 **`${{ github.actor }}`** aka ${{ steps.tagged.outputs.reviewer }} send message from 🎉
                                      🍻 Repository: ${{ github.repository }}      
```
#### Input variables 
* secrets.TELEGRAM_BOT - is token of your Telegram bot just saved in repository secrets with name `TELEGRAM_BOT`
* secrets.TELEGRAM_TO - is user/group/channel id you want bot send message to, just saved this ID in repository secrets with name `TELEGRAM_TO`
* format - Format of message in `markdown` or `html`. See [MarkdownV2 style](https://core.telegram.org/bots/api#markdownv2-style) 
* message - Your predefine message
