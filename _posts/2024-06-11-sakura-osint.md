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
