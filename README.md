index.js
```js
const { Client, Intents, MessageEmbed } = require('discord.js');
const { token, prefix } = require('./config.json');

const client = new Client({
    intents: [
        Intents.FLAGS.GUILDS,
        Intents.FLAGS.GUILD_MESSAGES,
    ]
});

client.on('ready', () => {
    console.log('the bot is ready')
})

client.on('messageCreate', message => {

    const args = message.content.trim().split(/ +/g);
    const cmd = args[0].slice(prefix.length).toLowerCase();

    if (cmd === 'hi') {
        message.reply('hello')
    }

    if (cmd === 'server') {

        let Embed = new MessageEmbed()
            .setColor('RED')
            .setAuthor(message.guild.name, message.guild.iconURL({ dynamic: true }))
            .setDescription(`Owner: <@${message.guild.ownerId}> | ${message.guild.ownerId}\nMembers: ${message.guild.memberCount}\nCreated at: ${message.guild.createdAt.toLocaleString()}`)
            .setThumbnail(message.guild.iconURL({ dynamic: true }))
            .setImage(message.guild.iconURL({ dynamic: true }))

        message.reply({ embeds: [Embed] })

    }

    if (cmd === 'user') {

        let target = message.mentions.users.first() || message.author;
        let member = message.guild.members.cache.get(target.id);

        let Embed = new MessageEmbed()
            .setColor('BLUE')
            .setFields(
                { name: 'username:', value: member.user.username },
                { name: 'ID:', value: member.user.id },
                { name: 'Tag:', value: `#${member.user.discriminator}` },
                { name: 'Mention:', value: `<@${member.user.id}>` },
                { name: 'Created at:', value: `${member.user.createdAt.toLocaleString()}` },
                { name: 'Joined at:', value: `${member.joinedAt.toLocaleString()}` },
            )
            .setThumbnail(member.user.avatarURL({ dynamic: true }))
            .setAuthor(member.user.tag, member.user.avatarURL({ dynamic: true }))
            .setFooter(`Requested by: ${message.author.username}`, message.author.avatarURL({ dynamic: true }))
            .setTimestamp()

        message.reply({ embeds: [Embed] })

    }

})

client.login(token);
```
