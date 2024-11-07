---
title: "Sakura Room Writeups - TryHackMe"
date: 2024-11-06
permalink: /posts/2024/10/spooky-ctf-wu/
tags:
  - OSINTDojo
  - ctf
  - cybersecurity
  - osint
---

Use a variety of OSINT techniques to solve the room created by the OSINT Dojo.

## Task 2

- I downloaded the image left by the attacker at [picture link](https://raw.githubusercontent.com/OsintDojo/public/3f178408909bc1aae7ea2f51126984a8813b0901/sakurapwnedletter.svg).
- Then I used exiftool to extract the picture informations:

<img src='/images/sakuraexif.png'>

- From the export-filename, I figured out that the attacker's username is: **SakuraSnowAngelAiko**

## Task 3

- Many people become attached to their usernames, and may therefore use it across a number of platforms, making it easy to find other accounts owned by the same person when the username is unique enough.
- I searched the attacker's username on google and was able to find a [github](https://github.com/sakurasnowangelaiko) account and a [twitter](https://x.com/sakuraloveraiko?lang=en) account with the same username.
- A post on their Twitter account reveals their main account which is [AiKOABE3](https://x.com/AiKOABE3). So their name must be **Aiko Abe**. <br>

- It was challenging to find the attacker's email. I found zero information about it on Twitter, then started searching through their Github account and found a [PGP key repo](https://github.com/sakurasnowangelaiko/PGP).
- When creating a PGP key, users may enter information such as a name, username, or email address which later can be extracted from their public key. So I copied their public key and import into Kleopatra to extract the informations from the key.

<img src='/images/sakuramail.png'>

- So their email is: **SakuraSnowAngel83@protonmail.com**

## Task 4

- There are multiple repos on their github account, one of them named ETH, so I guess it must be related to their crypto wallet. Looking at the current version of the [miningscript](https://github.com/sakurasnowangelaiko/ETH/blob/main/miningscript), there is nothing much. But looking at the history version of the file, I found their wallet address:

<img src='/images/sakurawallet.png'>

- Using their wallet address: **0xa102397dbeeBeFD8cD2F73A89122fCdB53abB6ef** on Blockchain, I can track their history transactions. The mining pool the attacker received payments from on January 23, 2021 UTCT was: **Ethermine**

<img src='/images/sakuraether.png'>

- The other cryptocurrency the attacker exchanged is: **Tether**

<img src='/images/sakuratether.png'>

## Task 5

- The attacker's current Twitter handle was already found at task 1: **SakuraLoverAiko**

<img src='/images/twittername.png'>

- From the attacker's posts on Twitter, it looks like they store they Regular wifi and passwords somewhere on the Dark Web.

<img src='/images/sakuraposts.png'>

- Their post on 23th Jan, 2021 emphasized that: "Anyone who wants them will have to do a real DEEP search to find where I PASTEd them". So the website must be something related to DEEP PASTE, isn't it?
- I actually found a website https://deepweblinks.net/pastebin/ using Tor, but the links provided by the website no longer work. So the last solution is to use the hint image provided.

<img src='/images/sakurahint.png'>

- The URL for the location where the attacker saved their WiFi SSIDs and passwords is: **http://deepv2w7p33xa4pwxzwi2ps4j62gfxpyp44ezjbmpttxz3owlsp4ljid.onion**

- Based on their home wifi information on DEEP PASTE: "DK1F-G Fsdf324T@@". I performed a search on [Wigle](https://wigle.net/) with SSID being DK1F-G and was able to find their location which is close to Hirosaki as well as BSSID: **84:af:ec:34:fc:f8**

<img src='/images/sakurawigle.png'>

## Task 6

- This is the last image they posted before they head home:

<img src='/images/sakura_lastpost.png'>

- At first, I was sure this is Japan because: 1/All of their informations lead to Japan and 2/I see a cherry blossom. I took a lot of time searching thourgh Japan airports but there was no result.
- It was not until when I changed the searching object to the below picture, the result return that it is actually the Washington Monument:

<img src='/images/dcmonument.png'>
<img src='/images/monumentif.png'>

- So I did a search for airports near the area and found Ronald Reagan Washington National Airport or **DCA**

- The rest questions are much more easier. Here are the other two posts I used to find the answer to other questions:

<img src='/images/haneda.png'>
<img src='/images/lake.png'>

- To find the last airport the attacker had their last layover in, I use Google with keyword: "JAL First Class Lounge Sakura Lounge" and there's only one airport that has the same lounge as in the picture which is Haneda airport - **HND**.
- The name of the lake is **Lake Inawashiro**:

<img src='/images/lakename.png'>

- The attacker's hometown is **Hirosaki** because we know the location of their home wifi.
