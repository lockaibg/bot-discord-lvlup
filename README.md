//systeme de lvl up
if(message.content){
    if(message.author.id != bot.user.id){
        if(!lvl[message.author.id+message.guild.id]){
            console.log(lvl[message.author.id+message.guild.id])
            lvl[message.author.id+message.guild.id] = 0
            lvl[message.author.id+message.guild.id+"lvl"] = 1
            lvl[message.author.id+message.guild.id+"nombre"] = 10
        }
    lvl[message.author.id+message.guild.id] = lvl[message.author.id+message.guild.id] + 1
    Savelvl()
    }
}
if(message.author.id != bot.user.id){
    if(lvl[message.author.id+message.guild.id] == lvl[message.author.id+message.guild.id+"nombre"]){
        message.channel.send(`<@${message.author.id}> est maintenant lvl \`${lvl[message.author.id+message.guild.id+'lvl']}\``)
        lvl[message.author.id+message.guild.id] = 1
        lvl[message.author.id+message.guild.id+"nombre"] = lvl[message.author.id+message.guild.id+"nombre"] + 10
        lvl[message.author.id+message.guild.id+"lvl"] = lvl[message.author.id+message.guild.id+"lvl"] + 1
        Savelvl()
}
}
if(message.content.startsWith(prefix+"lvl")){
    var mentionn = message.mentions.users.first()
    if(mentionn){
    var embedlvl = new Discord.MessageEmbed()
    .setTitle(`le lvl de \`${mentionn.username}\` est :`)
    .setDescription(lvl[mentionn.id+message.guild.id+"lvl"]-1)
    .addField("il lui manque : ", `\`${lvl[mentionn.id+message.guild.id]}/${lvl[mentionn.id+message.guild.id+"nombre"]}\` d'xp pour lvl up`)
    .setColor('#10F1D3')
    .setThumbnail(mentionn.displayAvatarURL())
    message.channel.send(embedlvl)
    }else{
        var embedlvl = new Discord.MessageEmbed()
        .setTitle(`le lvl de ${message.author.username} est :`)
        .setDescription(lvl[message.author.id+message.guild.id+"lvl"]-1)
        .setColor('#10F1D3')
        .addField("il lui manque : ", `\`${lvl[message.author.id+message.guild.id]}/${lvl[message.author.id+message.guild.id+"nombre"]}\` d'xp pour lvl up`)
        .setThumbnail(message.author.displayAvatarURL())
        message.channel.send(embedlvl)
    }
}
//set-lvlup
if(message.content.startsWith(prefix+'set-lvlup')){
    if(message.guild.member(message.author).hasPermission("MANAGE_ROLES")){
    if(!lvl[`roleup1${message.guild.id}`]||lvl[`roleup1${message.guild.id}`] == ""){
    grade = message.mentions.roles.first()
    niveau = message.content.split(' ').slice(2)
    console.log(niveau)
    if(!grade){
        message.channel.send(`utilise la commande comme ceci : ${prefix}set-lvlup <mention du role à donner> <niveau auxquels on obtient le role>`)
    }else if(!niveau){
        message.channel.send(`utilise la commande comme ceci : ${prefix}set-lvlup <mention du role à donner> <niveau auxquels on obtient le role>`)
    }else if(isNaN(niveau)){
        message.channel.send('tu dois mettre un chiffre pour le nombre de lvl (tu as peut être aussi mis 2 espaces entre la mention et le chiffre)')
    }else{
        lvl[`roleup${message.guild.id}`] = niveau
        lvl[`roleup1${message.guild.id}`] = grade
        Savelvl()
        message.channel.send(`à partir de maintenant quand une personne est lvl ${niveau} elle deviendra : ${grade}`)
    }}else{
        message.channel.send('tu as déjà un roleup paramétré')
    }
}else{
    message.channel.send("tu n'as pas la permission de faire ceci")
}
}
if(lvl[`${message.author.id}${message.guild.id}lvl`] - 1 == lvl[`roleup${message.guild.id}`]){
    if(message.author.id != bot.user.id){
        if(!lvl[`bolean${message.author.id}${message.guild.id}`]){
            message.channel.send(`${message.author.username} à gagné le role : ${lvl[`roleup1${message.guild.id}`]} car il est passé lvl ${lvl[`roleup${message.guild.id}`]}`)
            message.guild.member(message.author).roles.add(lvl[`roleup1${message.guild.id}`])
            lvl[`bolean${message.author.id}${message.guild.id}`] = true
        }    
    }
}
if(message.content == prefix+'delete-lvlup'){
    message.channel.send("j'ai bien suprimé la commande de lvlup")
    lvl[`roleup${message.guild.id}`] = ""
    lvl[`roleup1${message.guild.id}`] = ""
}
