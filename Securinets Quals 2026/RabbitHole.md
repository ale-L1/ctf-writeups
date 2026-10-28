# RabbitHole
**Category:** OSINT

## Overview

RabbitHole was an OSINT challenge that involved following a series of clues across different websites and platforms. The investigation started with a video game composer and gradually led to video game lore, social media posts, animation credits, and Japanese personal blogs.

Here is how I approached each question and the sources I used along the way.

---

### 1. "Identify the music composer for Outer Wilds"
A quick Google search revealed that Andrew Prahlow composed the soundtrack for the video game *Outer Wilds*.
This gave me the starting point for the rest of the investigation.

### 2. "The composer created music for a short film associated with a popular esports game. In the first minute of the film, a person is seen playing the game as a specific character. What is the name of the adopted mother of that character?"
The next clue mentioned that the composer had worked on a short film associated with a popular esports game.
I searched for short films involving Andrew Prahlow and came across [Godlike](https://www.youtube.com/watch?v=UyToOv6g2ZE), a short film related to *League of Legends*.
After looking into the film, I found a [Reddit discussion about it](https://www.reddit.com/r/leagueoflegends/comments/50bztb/godlike_a_league_film_league_of_legends/) that helped me identify the character as Riven.
From there, I looked into Riven's background and found that her adoptive mother is Shava Konte.

### 3. "Last year, the composer participated in a music event hosted at a university. What is the name of the location where the event was held?"
The third question referred to a music event hosted at a university in 2025.
I found information linking Andrew Prahlow to an event involving the Game Music Ensemble at the University of California, Los Angeles (UCLA).
In [this post on X](https://x.com/andrewprahlow/status/1919850338800325076), he shared information about the event.
The specific location was mentioned in the replies rather than in the original post, so I had to look beyond the main post to find the relevant detail. The event was held at the Ackerman Grand Ballroom.

<img width="537" height="672" alt="immagine" src="https://github.com/user-attachments/assets/2c40897a-ebdb-4740-b77f-e3a25d303b06" />

### 4. "What is the composer's best fruit?"
This one was more straightforward. I found an [older post by Andrew Prahlow on X](https://x.com/andrewprahlow/status/608525883809251328) in which he mentioned that his favourite fruit is kumquat.

### 5. "The composer contributed music to a Pokémon episode. Who was the Production Manager Assistant for that episode?"
Next, I searched for Andrew Prahlow's work on Pokémon.
His official website mentions that he contributed music to a short Pokémon episode. Searching for his name alongside terms related to the Pokémon short led me to [the episode on YouTube](https://www.youtube.com/watch?v=VCZUnl7eYlE).
I checked the credits at the end of the video and identified the Production Manager Assistant as Jun Aoki, along with the company they worked for.

<img width="560" height="98" alt="immagine" src="https://github.com/user-attachments/assets/f9bb4217-2f8b-493e-a140-2838e723432d" />

### 6. "Where was the Production Manager Assistant born, and when?"
I visited the official [Space Neko Company website](https://www.spaceneko.com/jp/static/about/) and translated the Japanese page into English.
After scrolling through the page, I found biographical information about Jun Aoki, including details about his birthplace and year of birth.
Clicking his name led me to his personal website, which provided another source to check against.

### 7. "In 2024, the Production Manager Assistant relocated their studio to a new location. Where did they relocate to?"
I returned to the Space Neko Company website to investigate the studio's relocation.
The information available on the company's website indicated that Space Neko was based in Musashino, Tokyo, in 2024.

### 8. "What is the blood type of the Production Manager Assistant?"
I revisited Jun Aoki's personal website and found a link to his blog.
The link led to his [Ameba blog](https://ameblo.jp/aokijun/).
On the right-hand side of the page, below his profile picture, I found his profile information, which included his blood type being A.

### 9. "On what date did the Production Manager Assistant mention that his phone had slipped out of his hand and broken, Ikebukuro Station?"
For the final question, I needed to find a specific entry in Jun Aoki's blog.
Since the blog was in Japanese, I translated the relevant search term for "mobile phone" into Japanese. The keyword I used was **携帯**.
Searching the blog with this term narrowed the results down to a smaller number of entries. One of them matched the incident described in the challenge, and it was posted the 10th of December 2005.
I found the relevant information in [this blog entry](https://ameblo.jp/aokijun/entry-10014009422.html).

## Conclusion

This challenge was a good introduction to OSINT because it required following clues across several different sources rather than relying on a single search.

I started with a video game soundtrack and eventually worked my way through social media posts, video credits, company websites, and a Japanese blog.

One useful takeaway was that the information needed to solve a question is not always on the page you initially find. Sometimes, checking replies, following links, translating search terms, and exploring related sources can make all the difference.
