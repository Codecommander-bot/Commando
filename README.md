🤖#sudo su 
```sh
git clone https://github.com/Codecommander-bot/Commando.git
cd Commando
```

#### 2. Install Dependencies
Install the required packages and dependencies. The `package.json` file typically lists these dependencies.
```sh
npm install
```

#### 3. Configure Environment Variables
Look for a `.env.example` file or similar, and rename it to `.env`. Edit the `.env` file to include your bot’s token and any other required environment variables.

Example `.env` file:
```plaintext
DISCORD_TOKEN=your-discord-bot-token
PREFIX=!
```

#### 4. Set Up the Coin Distribution Command
Add a command for distributing coins to users. You’ll need to create or edit the file in the `commands` directory.

Create or edit a file named `distributeCoins.js` in the `commands` directory:
```javascript
const Discord = require("discord.js");

module.exports = {
name: 'distribute_coins',
description: 'Distribute coins to all users',
execute(message, args) {
const amount = parseInt(args[0]);
if (isNaN(amount)) {
return message.reply('Please provide a valid number.');
}

const members = message.guild.members.cache.filter(member => !member.user.bot);
members.forEach(member => {
const userId = member.user.id;
if (!global.userBalances) global.userBalances = {};
global.userBalances[userId] = (global.userBalances[userId] || 0) + amount;
message.channel.send(`${member.user.username} got ${amount} coins!`);
});
}
};
```

#### 5. Create an Index File to Register Commands
Ensure your bot registers the new command and handles messages correctly.

Edit `index.js` or the main file for your bot:
```javascript
const fs = require('fs');
const Discord = require('discord.js');
const client = new Discord.Client();
require('dotenv').config();

client.commands = new Discord.Collection();

const commandFiles = fs.readdirSync('./commands').filter(file => file.endsWith('.js'));

for (const file of commandFiles) {
const command = require(`./commands/${file}`);
client.commands.set(command.name, command);
}

```# Commando