# Environment Config

# store your secrets and config variables in here
# only invited collaborators will be able to see your .env values

# reference these in your code with process.env.SECRET
TOKEN=NDg1NzU2NDY5MzUwODkxNTIy.DuGQPw.Ilev4TBuLju7vnhxED_Txy3Av6o
SECRET=UH32Rzv8gpSiffAUGQIpmcDiwsGDbA5Z
MADE_WITH=love

# note: .env is a shell file so there can't be spaces around =

body {
  margin: 0 0 0 0;
  font-size: 16px;
  font-family: "Arial", sans-serif;
  background: #ffffff;
  padding: 0 0 0 0;
  overflow: hidden;
}   
const Discord = require('discord.js')
const fs = require('fs')
const bot = new Discord.Client
const Config = require('./config.json')
const Core = require("./core.js");
const userData = JSON.parse(fs.readFileSync("./userData.json", "utf8"));
const create = new Set()
const userUsed = new Set()
const wait = new Set()
const test = new Set()
const study = new Set()
const items = JSON.parse(fs.readFileSync("./items.json", "utf8"));

const ownerId = ['478070941633478687', '501351710657347598']

bot.on("guildMemberAdd", async member => {
	let welcomeChannel = member.guild.channels.find(`name`, "welcome-goodbye");
	if (!welcomeChannel) return;
	welcomeChannel.send(`Welcome to the Server ${member}! Dont forget to read the rules! Enjoy!`);
  member.addRole(member.guild.roles.find(x => x.name === "User"));
	member.send("I hope you enjoy this server. Don't forget to read the rules!")
});

bot.on("guildMemberRemove", async member => {
	let welcomeChannel = member.guild.channels.find(`name`, "welcome-goodbye");
	if (!welcomeChannel) return;
	welcomeChannel.send(`Goodbye ${member}`);
	member.send("Why did you leave? Please come back. :slight_frown: ")
});

//Command Handler
bot.on('message', message => {
  
 
  let msg = message.content.toUpperCase();
  let prefix = Config.prefix
  let args = message.content.slice(prefix.length).trim().split(' ');
  let cmd = args.shift().toLowerCase();
  let chance = Math.floor(Math.random()*150 + 1)
  let sender = message.author
  
  if(message.author.bot) return
  
  
  if(!userData[message.author.id]) userData[message.author.id] = {money: 1000, videos: 0, revenue: 0, views: 1, computer: 1, quality: 1, content: 1, points: 0, subs: 0, member: false, found: false, found2: false, found3: false, iq: 1, games: 0}
  
  if(message.member.roles.find(`name`, `Membership`)){
    userData[sender.id].member = true
  }else {userData[sender.id].member = false}
  
  if(chance === 1){
    message.reply('You were the lucky one this time you got 1K cash!')
    userData[sender.id].money += 1000               
  }
  function gain() {
    if(userData[sender.id].videos === 0) return
    userData[sender.id].revenue += Math.floor(userData[sender.id].computer + userData[sender.id].videos + userData[sender.id].views)
  }
  setInterval(gain, 1000 * 60 * 60);
  function view() {
    if(userData[sender.id].videos === 0) return
    userData[sender.id].views += Math.floor(userData[sender.id].videos * userData[sender.id].quality)
  }
  setInterval(view, 5000 * 60);
  function sub() {
    userData[sender.id].subs += Math.floor(userData[sender.id].videos + userData[sender.id].views / 10)
  }
  setInterval(sub, 1000 * 60 * 60);
  function cash() {
    if(userData[sender.id].member === true){userData[sender.id].money += 10000}
  }
  setInterval(cash, 8.64e+7);
  function game() {
    if(userData[sender.id].games === 0) return
    userData[sender.id].money += Math.floor(userData[sender.id].games * 10)
  }
  setInterval(game, 1000 * 60 * 60 * 2)
  
  if(!message.content.startsWith(prefix)) return
  if(userUsed.has(message.author.id)) return message.channel.send('Wait a second before you can use this!')
   
  if(msg === `${prefix}BALANCE` || msg === `${prefix}BAL`){
     message.channel.send(`**Your Balance Is**: ${userData[message.author.id].money}`)
   }
  
  if(msg === `${prefix}SERVERS`){message.channel.send(`I am in ${bot.guilds.size} servers`)}
  
  if(msg.startsWith(`${prefix}DM`)){
    let player = message.guild.member(message.mentions.users.first())
    if(!player){return message.reply('could not find player')}
    if(!args[1]){return message.reply('please make sure you send a message')}
    player.send(`sent by ${sender}: ${args.join(" ").slice(22)}`)
    message.channel.send(`DMed ${player} ${args.join(" ").slice(22)}`)
  }
//DO NOT OPEN SECRET UPDATE WHEN WE GET TO 50 SERVERS
  {
  if(msg.startsWith(`${prefix}COPYRIGHT`)){
    if(wait.has(sender.id)) return message.reply('Wait 24 hours before you can copyright again')
    let player = message.guild.member(message.mentions.users.first())
    if(!player) return message.channel.send('could not find player!')
    let reson = args.join(" ").slice(22)
    if(!reson) return message.channel.send('The reason does not effect the outcome but its cool to have one!(just spam some leters in it or something)')
    
    let chance = Math.floor(Math.random() * 10 + userData[player.id].content)
    if(!userData[player.id]) userData[player.id] = {money: 1000, videos: 0, revenue: 0, views: 1, computer: 1, quality: 1, points: 0, subs: 0, member: false, found: false, found2: false, found3: false}
    if(sender.id === ownerId[0]) {
      chance = 1 
      message.channel.send('hello powermins')
    }
    if(chance === 1){
      message.channel.send({embed:{
        title: "COPYRIGHT",
        color: 0xFF0000,
        fields: [{
          name: `Copyrighted ${args[0]}`,
          value: `${sender} Copyrighted ${player}, with the id of ${player.id}, for the reason "${reson}". ${player} was sued and %50 of their videos, along with %25 of their cash, was claimed by ${sender}`
        }]
      }})
      player.send(`You have been copyrighted by ${sender} for the reason "${reson}" They claimed %50 of your videos and %25 of your cash.`)
      userData[sender.id].money += Math.floor(userData[player.id].money / 4)
      userData[player.id].money -= Math.floor(userData[player.id].money / 4)
      userData[sender.id].videos += Math.floor(userData[player.id].videos / 2)
      userData[player.id].videos -= Math.floor(userData[player.id].videos / 2)
    }else{
      message.channel.send(`You failed to copyright ${player} and lost the sue. You had to pay then %10 of what you own`)
      userData[sender.id].money -= Math.floor(userData[sender.id].money / 10)
      userData[player.id].money += Math.floor(userData[sender.id].money / 10)
      userData[sender.id].videos -= Math.floor(userData[sender.id].videos / 10)
      userData[player.id].videos += Math.floor(userData[sender.id].videos / 10)
    }
     wait.add(message.author.id);
      setTimeout(() => {
          wait.delete(message.author.id);
        }, 8.64e+7);
    
  }
  if(msg.startsWith(`${prefix}VLOG`)){
    if(userData[sender.id].member === true){
    if(create.has(sender.id)) return message.channel.send('Wait 1 hour before vloging!')
    }else {if(create.has(sender.id)) return message.channel.send('Wait 2 hours before vloging!')}
    userData[sender.id].videos ++
    userData[sender.id].subs += 25
    message.channel.send('You created a youtube video, you now have ' + userData[sender.id].videos + ' videos. You also gained 25 subs from your vlog!')
    if(userData[sender.id].member === true){
      create.add(message.author.id);
      setTimeout(() => {
          create.delete(message.author.id);
        }, 1000 * 60 * 60);
    }else{
      create.add(message.author.id);
      setTimeout(() => {
          create.delete(message.author.id);
        }, 1000 * 60 * 60 *2);}
  }
  if(msg.startsWith(`${prefix}CHALLENGE`)){
    if(userData[sender.id].member === true){
    if(create.has(sender.id)) return message.channel.send('Wait 1 hour and 30 minutes before doing a challeng video!!')
    }else {if(create.has(sender.id)) return message.channel.send('Wait 3 hours before doing a challenge video!')}
    userData[sender.id].videos ++
    userData[sender.id].subs += 30
    message.channel.send('You created a youtube video, you now have ' + userData[sender.id].videos + ' videos. You also gained 30 subs from your challenge!')
    if(userData[sender.id].member === true){
      create.add(message.author.id);
      setTimeout(() => {
          create.delete(message.author.id);
        }, 1000 * 60 * 60 + 1000 * 60 * 3);
    }else{
      create.add(message.author.id);
      setTimeout(() => {
          create.delete(message.author.id);
        }, 1000 * 60 * 60 *3);}
  }
  if(msg.startsWith(`${prefix}STUDY`)){
    if(study.has(sender.id)) return message.channel.send('Wait 1 hour before studying again!')
    userData[sender.id].iq += 5
    message.channel.send(`You studied and gained 5 IQ your iq is now at ${userData[sender.id].iq}`)
    study.add(message.author.id);
      setTimeout(() => {
          study.delete(message.author.id);
        }, 1000 * 60 * 60);
  }
  if(msg.startsWith(`${prefix}NEWGAME`)){
    if(userData[sender.id].iq < 50){return message.channel.send('You cant create a game yet you dont have a high enough iq You must have atlease 50 IQ, study study study!')}
    if(test.has(sender.id)) return message.channel.send('You must wait 24 hours before creating a game!')
    if(userData[sender.id].money < 10000) return message.channel.send('You need 10K money to create a game!')
    message.channel.send(`You created a game! This did make you lose 10 IQ. After 24 hours you will be able to create another game!`)
    userData[sender.id].games ++
    userData[sender.id].iq -= 10
    userData[sender.id].subs += 100
    userData[sender.id].money -= 10000
    test.add(message.author.id);
      setTimeout(() => {
          test.delete(message.author.id);
        }, 8.64e+7);
  }
  if(msg === `${prefix}IQ`){
    message.channel.send(`Your IQ is: ${userData[sender.id].iq}`)
  }
  if(msg === `${prefix}GAMES`){
    message.channel.send(`You have ${userData[sender.id].games} games`)
  }
  }//also please do not open the updates.json
//DO NOT OPEN SECRET UPDATE WHEN WE GET TO 50 SERVERS
  if(msg.startsWith(`${prefix}CREATE`)){
    if(userData[sender.id].member === true){
    if(create.has(sender.id)) return message.channel.send('Wait 30 minutes before creating another video.')
    }else {if(create.has(sender.id)) return message.channel.send('Wait 1 hour before creating another video.')}
    userData[sender.id].videos ++
    message.channel.send('You created a youtube video, you now have ' + userData[sender.id].videos + ' videos.')
    if(userData[sender.id].member === true){
      create.add(message.author.id);
      setTimeout(() => {
          create.delete(message.author.id);
        }, 1000 * 60 * 60 / 2);
    }else{
      create.add(message.author.id);
      setTimeout(() => {
          create.delete(message.author.id);
        }, 1000 * 60 * 60);}
  }
  
  if(msg === `${prefix}$&^@GY!0&`){
    if(userData[sender.id].found === false){
    message.channel.send('you found a secret tho im not sure how you did but you did! enjoy your 50K Cash!')
    userData[sender.id].money += 50000
      userData[sender.id].found = true
    }else if(userData[sender.id].found === true){return}
  }
  
    if(msg === `${prefix}SUBBOT`){
    if(userData[sender.id].found2 === false){
    message.channel.send('You found a secret enjoyyour 10K')
    userData[sender.id].money += 10000
      userData[sender.id].found2 = true
    }else {return}
  }
  
      if(msg === `${prefix}POWERMINS`){
    if(userData[sender.id].found3 === false){
    message.channel.send('You found a secret enjoy your 10K!')
    userData[sender.id].money += 10000
    userData[sender.id].found3 = true
    }else {return}
  }
  if(msg === `${prefix}SHADOW`){
    if(userData[sender.id].found3 === false){
    message.channel.send('You found a secret enjoy your 10K!')
    userData[sender.id].money += 10000
    userData[sender.id].found3 = true
    }else {return}
  }
  
   if(msg.startsWith(prefix + "PURGE")) {
     async function p(){
    // This command removes all messages from all users in the channel, up to 100.
    if (!message.member.hasPermission("MANAGE_MESSAGES")) return message.reply("You do not have permission! \n you need manage messages");
    // get the delete count, as an actual number.
    const deleteCount = parseInt(args[0], 10);
    
    // Ooooh nice, combined conditions. <3
    if(!deleteCount || deleteCount < 2 || deleteCount > 100)
      return message.reply("Please provide a number between 2 and 100 for the number of messages to delete");
    
    // So we get our messages, and delete them. Simple enough, right?
    const fetched = await message.channel.fetchMessages({limit: deleteCount});
    message.channel.bulkDelete(fetched)
      .catch(error => message.reply(`Couldn't delete messages because of: ${error}`));
     }
     p()
  }
  
  if(msg === `${prefix}CLICKBAIT`){
    if(userData[sender.id].member === true){
    if(create.has(sender.id)) return message.channel.send('Wait 30 minutes before creating another video.')
    }else {if(create.has(sender.id)) return message.channel.send('Wait 1 hour before creating another video.')}
     message.channel.send('You created a clickbaited video. But dont use this too much. you lose 50 subs everytime you do!')
      userData[sender.id].clickbait += 1
    userData[sender.id].videos += 1 
    if(userData[sender.id].subs < 50){userData[sender.id].subs = 0}else {
      userData[sender.id].subs -= 50
    }
    if(userData[sender.id].member === true){
      create.add(message.author.id);
      setTimeout(() => {
          create.delete(message.author.id);
        }, 1000 * 60 * 60 / 2);
    }else{
      create.add(message.author.id);
      setTimeout(() => {
          create.delete(message.author.id);
        }, 1000 * 60 * 60);}
  }
  
  if(msg === `${prefix}RESTART`){
    userData[sender.id].money = 1000
    userData[sender.id].views = 0
    userData[sender.id].videos = 0
    userData[sender.id].revenue = 0
    userData[sender.id].computer = 1
    userData[sender.id].quality = 1
    userData[sender.id].clickbait = 0
    userData[sender.id].subs = 0
    userData[sender.id].iq = 0
    userData[sender.id].games = 0
    userData[sender.id].found3 = false
    create.delete(message.author.id)
    study.delete(message.author.id)
    test.delete(message.author.id)

    message.channel.send('You started a new channel, Its time for a new life.')
  }
  
  if(msg === `${prefix}COLLECT`){
    userData[sender.id].money += userData[sender.id].revenue
    userData[sender.id].revenue = 0
    message.channel.send('You collected your revenue.')
  }
  
    if(msg === `${prefix}BUYLAMBORGINI`){
     userData[sender.id].money -= 99999999
    message.channel.send('You wasted your moneyz on lamborginzi')
  }
  
  if(msg === `${prefix}REVENUE`){
    message.channel.send(`Your revenue is: ${userData[sender.id].revenue}`)
  }
  
  if(msg === `${prefix}VIEWS`){
    message.channel.send(`Your current views are: ${userData[sender.id].views}`)
  }
  
  if(msg === `${prefix}CHEAT`){
    if(sender.id === ownerId[0] || sender.id === ownerId[1] || sender.id === ownerId[2]){
      userData[sender.id].money = 99999999
      userData[sender.id].videos = 99999999
      create.delete(message.author.id)
      userUsed.delete(message.author.id)
      userData[sender.id].revenue = 99999999
      userData[sender.id].views = 9999999
      userData[sender.id].clickbait = 0
      userData[sender.id].subs = 99999999
      userData[sender.id].computer = 5
      userData[sender.id].quality = 10
      userData[sender.id].member = 0
      userData[sender.id].points = 99999999
      userData[sender.id].iq = 99999999
    }else message.channel.send("You are not owner.") 
  }
  
    if(msg === `${prefix}BYPASSTIMES`){
    if(sender.id === ownerId[0] || sender.id === ownerId[1] || sender.id === ownerId[2]){
    create.delete(message.author.id)
    study.delete(message.author.id)
    test.delete(message.author.id)
    wait.delete(message.author.id)
    message.channel.send("Deleted your wait times.")
    }else message.channel.send("You are not a owner. Sorry")
  }
  

  
  if(msg.startsWith(`${prefix}SETCASH`)){
    if(message.author.id === ownerId[0] || sender.id === ownerId[1]){
    let player = message.guild.member(message.mentions.users.first())
    if(!player) return
    userData[player.id].money = parseInt(args[1])
    }else message.channel.send("You are not a owner.")
  }
    
   const commands = JSON.parse(fs.readFileSync('help.json', 'utf8'));
  
  if(msg.startsWith(prefix + "HELP")){
      
      if(msg === prefix + "HELP FUN"){
      
       const embed = new Discord.RichEmbed()
       .setColor(0xFFff00)
         
         let commandsFound = 0;
      
      for(var smd in commands) {
        
        if(commands[smd].group.toUpperCase() === "FUN"){
          commandsFound++
          embed.addField(`${commands[smd].name}`, `Description: ${commands[smd].desc} \n Usage: ${commands[smd].usage}`)
        }
        
        
      }
      embed.setTitle(`Fun Commands`);
      embed.setFooter(`Viewing fun commands`)
      embed.setDescription(`${commandsFound} commands found!`)
      
      message.author.send(embed)
      message.channel.send('Sent you a DM containing the commands!')
      }
    else if(msg === prefix + "HELP MOD"){
      
       const embed = new Discord.RichEmbed()
       .setColor(0xFFff00)
         
         let commandsFound = 0;
      
      for(var smd in commands) {
        
        if(commands[smd].group.toUpperCase() === "MOD"){
          commandsFound++
          embed.addField(`${commands[smd].name}`, `Description: ${commands[smd].desc} \n Usage: ${commands[smd].usage}`)
        }
        
        
      }
      embed.setTitle(`Mod Commands`);
      embed.setFooter(`Viewing Mod commands`)
      embed.setDescription(`${commandsFound} commands found!`)
      
      message.author.send(embed)
      message.channel.send('Sent you a DM containing the commands!')
      }
    
    else {
      let helpEmbed = new Discord.RichEmbed()
			.setColor("#30ffd5")
			.setTitle("Please select a category")
			.addField("Categories:", "Fun \n Mod")
			.addField("How to:", "Type `!help [category]` to see the commands \n \n For any extra information about a command, do: `>help [command]`");

		message.channel.send(helpEmbed);
    }
   }

  if(msg.startsWith(`${prefix}BUY`)){
    if(msg === `${prefix}BUY`){
    const embed = new Discord.RichEmbed()
       .setColor(0xFF0000)
         
         let commandsFound = 0;
      
      for(var smd in items) {
        
        if(items[smd].group.toUpperCase() === "ECO"){
          commandsFound++
          embed.addField(`${items[smd].name}`, `Description: ${items[smd].desc} \n Price: ${items[smd].price}`)
        }
     
      }
      embed.setFooter(`Use command !buy <item name> to buy a item`)
      embed.setDescription(`${commandsFound} items found!`)
      
      message.channel.send(embed)
  }else {
    for(var i in items){
      let itemName = items[i].name
      let itemPrice = items[i].price
      if(userData[message.author.id].money < itemPrice) return message.channel.send( `you dont have enough money for this!`)
    }
    if(args[0].toUpperCase() === "COMPUTER"){
      if(userData[sender.id].computer >= 5){return message.channel.send('This is at max level!')}
      userData[sender.id].computer += 1
      userData[sender.id].money -= 5000
      message.reply('You upgraded your computer to level ' + userData[sender.id].computer + '!')
    }
    else if(args[0].toUpperCase() === "QUALITY"){
      if(userData[sender.id].quality >= 10){return message.channel.send('This is at max level!')}
      userData[sender.id].quality += 1
      userData[sender.id].money -= 7500
      message.reply('You upgraded your quality to level ' + userData[sender.id].quality + '!')
    }
    else if(args[0].toUpperCase() === "CONTENT"){
      return message.channel.send("You upgraded your Content, Before it was trash!")
      if(userData[sender.id].content >= 5){return message.channel.send('This is at max level!')}
      userData[sender.id].content += 1
      userData[sender.id].money -= 8000
      message.reply('You upgraded your content to level ' + userData[sender.id].content + '!')
    }
    else{message.channel.send('thats not a valid item')}
  }
  }
  if(msg === `${prefix}SHOP`){
    const embed = new Discord.RichEmbed()
       .setColor(0xFF0000)
         
         let commandsFound = 0;
      
      for(var smd in items) {
        
        if(items[smd].group.toUpperCase() === "ECO"){
          commandsFound++
          embed.addField(`${items[smd].name}`, `Description: ${items[smd].desc} \n Price: ${items[smd].price}`)
        }
     
      }
      embed.setFooter(`Use command !buy <item name> to buy a item`)
      embed.setDescription(`${commandsFound} items found!`)
      
      message.channel.send(embed)
  }

  if(msg.startsWith(`${prefix}PAY`)){
    let player = message.guild.member(message.mentions.users.first() || message.guild.members.get(args[0]));
    if(!player) return message.channel.send('Could not find player.')
    if(!args[1]) return message.channel.send('Please state a amount')
    if(args[1] > userData[sender.id].money) return message.channel.send('You dont have that much to give!')
    userData[sender.id].money -= parseInt(args[1])
    userData[player.id].money += parseInt(args[1])
    message.channel.send(`You payed ${player} ${args[1]}`)
//XD  
  }
  
  if(msg === `${prefix}POINTS`){
    message.channel.send(`You have ${userData[sender.id].points} points.`)
  }
  
  if(msg === `${prefix}CHANNEL`){
    const embed = new Discord.RichEmbed()
    .setTitle(`${message.author.username}'s profile`)
    .setColor(0x6F5314)
    .setThumbnail(message.author.avatarURL)
    if(sender.id === ownerId[1]){embed.addField(`Did you know:`, ***♢The Gladiator♢#3105*** `is one of the 2 creators that made me?`)}
    if(sender.id === ownerId[0]){embed.addField(`Did you know:`, ***Hackode007#4799*** `is one of the 2 creators that made me?`)}
    embed.addField(`Level: `,`1`)
    .addField(`Cash: `, `${userData[sender.id].money}`)
    .addField(`Videos`, `${userData[sender.id].videos}`)
    .addField(`Subs`, `${userData[sender.id].subs}`)
    .addField(`Is Member`, `${userData[sender.id].member}`)
    
    message.channel.send(embed)
  }
  

  try{

    delete require.cache[require.resolve(`./commands/${cmd}.js`)]

    let ops = {
      ownerId: ownerId[0],
      coownerId: ownerId[1]
    }

    let commandFile = require(`./commands/${cmd}.js`);
    commandFile.run(bot, message, args, ops)

  }catch(e){
    //console.log(e.stack)
  }
  userUsed.add(message.author.id);
        setTimeout(() => {

          userUsed.delete(message.author.id);
        }, 1000);
  
fs.writeFile('userData.json', JSON.stringify(userData), (err) => {
        if (err) console.error(err);
      })
})

bot.on("ready", async () =>{
  console.log(`${bot.user.username} is online It's running on ${bot.guilds.size} servers!`);
  bot.user.setActivity(`${bot.guilds.size} servers - !help`, {type: "WATCHING"});
})

bot.login(process.env.TOKEN)
