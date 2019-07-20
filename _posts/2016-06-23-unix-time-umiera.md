---
layout: post
title: Unix time umiera
type: post
categories:
- Uniwersalne
tags:
- Unix Epoch
- unix timestamp
permalink: "/unix-time-umiera/"
image: 'https://sienicki.dev/assets/tiles/doesnotexists.jpg'
category: 'blog' 
introduction: Unix time - to taki magiczny czas liczony od początku istnienia epoki linuxa
---
Niby nic odkrywczego, ale to moja refleksja na dzisiaj :smile:

Unix time - to taki magiczny czas liczony od początku istnienia epoki linuxa, czyli 1 stycznia 1970 roku. Jest ograniczony - bo jego maksymalna wartość to 2147483647, co po przeliczeniu sekund daje datę 2038-01-19T03:14:07. 

Łatwo policzyć, że za 22 lata epoka linuxa nie będzie już dłużej mierzalna na 32 bitach.

Na co dzień się o tym nie myśli, a wiele API używa&nbsp;daty właśnie w takim formacie, ba... sam nawet przyczyniłem się niedawno do poszerzenia grona takich aplikacji&nbsp;idąc na ustępstwo robociarzom :wink:

No i jeszcze pozostaje sprawa z czytelnością takiego zapisu :tongue:

Upraszczajmy sobie życie, nie na odwrót :loudspeaker:

Zobacz również:
[Data jako liczba unix](https://sienicki.dev/data-jako-liczba-unix/)

