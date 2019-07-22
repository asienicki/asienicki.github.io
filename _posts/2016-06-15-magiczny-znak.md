---
layout: post
title: Magiczny znak
type: post
categories:
- Bez kategorii
- C#
tags:
- biały znak
- retrofit
- stackoverflow
- webapi
permalink: "/magiczny-znak/"
image: 'https://sienicki.dev/assets/tiles/char.jpg'
category: 'blog' 
introduction: krótka refleksja na temat kopiowania kodu
---
Dzisiaj krótka refleksja na temat kopiowania kodu z "Internietów".

Zalety? Oczywista oczywistość - **działa**.

Wady? **Nie wszystko**.

Ostatnio się naciąłem na magiczny znak który wyglądał jak spacja. 
Skopiowałem wkleiłem i publish na test, no bo co kosmicznego może być w poniższej linijce? :smiling_imp:
`DateTimeFormat = "yyyy-MM-dd HH:mm:ss";`  
Odpaliliśmy mobilkę i nie działa :anguished: 
Podpinamy konsolę z logami i okazuje się, że retrofit nie może sobie poradzić z moją datą... ale jak to?

Bez nadziei podejrzałem  znaki w komunikacie i znowu się zdziwiłem - zamiast spacji znalazłem jakiś chiński biały znak. Koniec.

#strolowanyPrzezStackOverflow :scream:

