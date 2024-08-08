# belki size lazım olur ben kullanmicam:

const Discord = require('discord.js')
const { permissionsEmbed, TickEmbed } = require('../../client/embed.js');

module.exports = {
name: 'sunucubilgi',
aliases: ['sunucu-bilgi', 'serverinfo', 'server-info', 'server', 'sunucu'],

async execute(client, message, args) {

    const {
        members,
        channels,
        emojis,
        roles,
        stickers
    } = message.guild;
    
    const owner = message.guild.members.cache.get(message.guild.ownerId);
    const thumbnail = message.guild.iconURL({size: 2048}) || client.user.avatarURL({size: 2048})
    const botCount = members.cache.filter(member => member.user.bot).size;
    const kullanıcılar = await message.guild.members.fetch();

    const textChannels = message.guild.channels.cache.filter(channel => channel.type === Discord.ChannelType.GuildText).size;
    const voiceChannels = message.guild.channels.cache.filter(channel => channel.type === Discord.ChannelType.GuildVoice).size
    const bannerURL = message.guild.bannerURL({ size: 2048 }) || null;
    
    const embed = new Discord.EmbedBuilder()
    .setColor(process.env.MAIN_COLOR)
    .setThumbnail(thumbnail)
    .setDescription(`# <:roleplayinggame:1267136769736970310> ${message.guild.name} | Hakkında Bilgi`)
    .addFields(
    { name: '<:premium_crown:1269624671796727819> Kurucu:', value: `${owner}`, inline: true },
    { name: '<:copy:1266882028494782535> Kuruluş Tarihi:', value: `<t:${parseInt(message.guild.createdTimestamp / 1000)}:R>`, inline: true },
    { name: `\u200B`, value: `\u200B`, inline: true },
    { name: '<:roles6:1267139741632040970> Üyeler:', value: `<:group:1267136715982770281> ${message.guild.memberCount - botCount}\n<:Discord_Bots:1269626330245496974> ${botCount}`, inline: true },
    { name: `<:text:1266882169499029625> Kanal Sayısı:`, value: `<:discord_channel:1267138267959001151> ${textChannels}\n<:voice_channel1:1267138316218794065> ${voiceChannels}`, inline: true },
    { name: `\u200B`, value: `\u200B`, inline: true },
    { name: '<:Zetry_invite:1269627955248762910> Özel Davet:', value: `${message.guild.vanityURLCode || "Bulunmuyor."}`, inline: true },
    { name: `<:server_boost:1269629704957071424> Takviye:`, value: `${message.guild.premiumTier ? `Seviye: ${message.guild.premiumTier}` : 'Bulunmuyor.'}`, inline: true },
    { name: `\u200B`, value: `\u200B`, inline: true },
    )

    if (bannerURL) {
        embed.setImage(bannerURL);
    }

    await message.channel.send({ embeds: [embed], components: [] })

},
};
