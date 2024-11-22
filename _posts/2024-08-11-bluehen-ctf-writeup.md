---
title: "Bluehens CTF 2024 Writeups"
date: 2024-11-08
permalink: /posts/2024/10/bluehen-ctf/
tags:
  - ctf
  - cybersecurity
  - OSINT
---

Writeup for OSINT questions I solved in Bluehens CTF 2024

Links: <br>
CTF Platform: (https://bluehens.ctfd.io/)[https://bluehens.ctfd.io/]

## Open-Source Intelligence

### Training Problem: Intro to OSINT

<img src='/images/introosint.png'>
<img src='/images/house.png'>

- By revere image searching on Google, I found out that this mansion belong to **Marc Ecko**.
- I then start searching for cars that owned by Marc and found this website through the google results: (https://www.autoevolution.com/news/marc-ecko-has-a-gurkha-video-29099.html)[https://www.autoevolution.com/news/marc-ecko-has-a-gurkha-video-29099.html]

- A video in this article show the license plate of the Gurkha that he owns which is: **WLJ80F** <br>

Flag: **udctf{marcecko_wlj80f}**

### I’m Hungry

- This question was actually a very tough one for me, but it was very satisfy solving it.

<img src='/images/hungry.png'>
<img src='/images/fball.jpg'>

- There is a text given inside the image “bswmrsvpjqtlbebwgawpnouxmtlpgjwfwbjswyj”. I put this text on dcode saw that it was a Vigenere Cipher. But in order to decode the Vigenere Cipher, I need to know the key/password.
- I use exiftool, binwalk and strings on the image to see if there any informations hidden inside the image. And strings return some information about a book:

<img src='/images/book1.png'>

- I then used Google to find information about the book [Il vero modo di scrivere in cifra con facilita, prestezza et securezza](https://books.google.ca/books/about/Il_vero_modo_di_scrivere_in_cifra_con_fa.html?hl=it&id=dqNNAAAAcAAJ&redir_esc=y)
- The text "fine pagina 20" means "end of page 20". So I accessed the book online and go to page 20 of the book, finding the keyword: "ILFINE".

<img src='/images/book2.png'>

- I then decoded the text “bswmrsvpjqtlbebwgawpnouxmtlpgjwfwbjswyj” with keyword "ILFINE" and got the result: “threeoneeighttwotwoeighteightfourtwoone”.
- I then searched the numbers "3182288421" on Google, and a chiken restaurant named Bayou Soul showed up. This was the point where I got stuck and did not know what to do. I tried the name of the restaurant as the flag but it was not correct.
- After wasting a lot of time and nearly gave up, I discover that the name of the image file given play an important role in this question: "a_paper_football_player".
- In paper football, a football player is also called "Flicker". I used (flickr)[https://www.flickr.com/] to search for "Bayou Soul". An account name Bayou Soul which was created on 11 November showed up:

<img src='/images/bayou.png'>

Flag: **udctf{7H@-w4SN7-S0-H4rd}**
