---
title:  "The Whimsy World of Unicode 🙅"
date:   2020-06-12 10:03:00 +0530
last_modified_at: 2020-08-13 11:41:00 +0530
permalink: working-with-unicode-and-emoji-in-csharp
description: Here's a detailed tutorial on how to work with unicode and emoji in in XML with c#
tags: [dotnet]
---
**Unicode**, can't live with them, can't live without them.

Emojis are a delightful way to express ourselves, but they can be bit of a pain to manage in code sometimes. I recently stumbled upon this issue of "*illegal character found*" while trying to deserialize an XML with utf-8 declared XML, which contained Unicode characters.

The solution was to use XmlReaderSetting, with CheckCharacters set to false and then use it in deserialization.

```
XmlReaderSettings readerSettings = new XmlReaderSettings() 
{ 
    CheckCharacters = false
};
```
Now, using this reader settings, the deserialization of XML with occasional Unicode was done just fine.

```
public static Player Deserialize(Stream request)
{
    Player player = null;

    XmlReaderSettings readerSettings = new XmlReaderSettings() 
    { 
        CheckCharacters = false
    };    
    XmlSerializer serializer = new XmlSerializer(typeof(Player));

    using(var reader = XmlReader.Create(new StreamReader(request, Encoding.UTF8),readerSettings))
    {
        customer = (Player)serializer.Deserialize(reader);
    }

    return player;
}
```
