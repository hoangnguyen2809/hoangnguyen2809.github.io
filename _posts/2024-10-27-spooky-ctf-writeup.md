---
title: "Spooky CTF Writeups"
date: 2024-10-27
permalink: /posts/2024/10/spooky-ctf-wu/
tags:
  - SpookyCTF 2024
  - ctf
  - cybersecurity
---

# Spooky CTF 2024 Write-ups

Links:
CTF Platform: https://spookyctf.ctfd.io/

- This is the second CTF challenge that I participated along with my SFU fellas. We have been participating in the CTF challenge since early October to prepare for the Canada Cyber ​​​​Challenge Regional which took place on November 23. This time, we ranked 96/832.
- This write-up contains the questions I was able to solve during the challenge. I have created this write-up to save and use for reference for future challenges.

## Open-Source Intelligence

1. among-us

<img src='/images/amongus.png'>

- I use Google to find all the information about the Jersey Devil and video games that Jersey Devil appears in as a character.
- Using Google was not very effective, I only found the game called "the devil's due" which is a really old game. Although Jersey Devil appears as an character, the game was not what I looked for since the hint said that this game has a sequeal that will be release this year.
- I use other AI tools such as ChatGPT or Bing, a name of a shop called "Lucky Pawn" shows up. This helped me to find the name of the game: The Wolf Among Us. <br>

Flag: **NICC{The_Wolf_Among_Us_Lucky_Pawn}**

2. perfect-nba-fit

<img src='/images/perfectnbafit.png'>

- Based on the researches I did in another question, I know that in New Jersey, there are creatures such as the Jersey Devil, Bigfoot, Chupacabra,... In this case Bigfoot seems like a perfect fit for NJIT because they are looking for a taller player.
- The question also mention that the information about the sighting should be class A.
- Using AI tools, I was able to find out this website: "https://www.bfro.net/ " , containing information about sighting events of Bigfoot. After filtering out the region of the sighting as well as the class, I found the solution for this question. <br>

Flag: **NICC{20_18:40_Route-563_Chatsworth}**

## Miscellaneous

1. Well-met

<img src='/images/wellmet.png'>

- The hint of the question is to read the lore at "https://spookyctf.ctfd.io/lore". I went to the site and read through all information of the characters there. A part of the answer was hidden inside a character's information box, with black color character and black background. I was able to see it when highlighting the text. The first part of the asnwer is: `NICC{StOr`
- Using web inspection, I found out that the element contain the above part is following:
  `<p class="redacted" id="spookyflag_part1">NICC{StOr</p>`

- From then, I all the element that has id related to flag, spookyflag, p2, p3,... and got:
  `<img class="img-fluid" src="/files/img/nah4real.png" width="400" height="400" id="spookyflag_p2" alt="IeS_DoNt_M"/>`
  `<span class="p-l redacted" id="p3">aKe_ThE_cTf_T</span>`
  `<!--<p4>oO_cTfY_rIgHt?}</p4> -->`
  <br>

Flag: **NICC{StOrIeS_DoNt_MaKe_ThE_cTf_ToO_cTfY_rIgHt?}**

## Steganography

1. devil's-secret-stash

2. set-your-intentions-right

3. Phenominal-Photo

4. whispers-in-morse

<img src='/images/morse.png'>

- A png file was given, I tried using tools such as exiftool, binwalk, file and strings to get information about this png.
- `strings` given me the last line: `Password: M.A.__.R.Y`
- Knowing the password, I use steghide to extract decrypt data from the image using the password:
  `steghide extract -sf MaryMorse.jpg -p M.A.__.R.Y -xf output.txt`

Flag: **NICC{tHe_whIspeRz_iN_Th3_aiR}**

## Web

1. cryptid-hunters

<img src='/images/crytidhunter.png'>

- An IP was given that lead to a very simple website with login function. So the first thing came to my mind was SQL Injection.
- I tried a basic payload: `' OR 1 -- -` and successfully logged into the site
- Useful SQL injection payload resource: https://github.com/payloadbox/sql-injection-payload-list

Flag: **NICC{1N_PuRSu1T_0F_4LL13S}**
