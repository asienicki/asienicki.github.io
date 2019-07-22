---
layout: post
title: Zaginiony commit
type: post
categories:
- Tools
tags:
- checkout
- commit
- GIT
- reflog
- SOURCE TREE
- utracony commit
- zaginiony commit
- zgubiony commit
- zmiana
permalink: "/zaginiony-commit/"
image: 'https://sienicki.dev/assets/tiles/commit.jpg'
category: 'blog' 
introduction: Po pierwsze... jak to zaginiony?
---
Po pierwsze... jak to zaginiony? :open_mouth:

Prosto dość :stuck_out_tongue_winking_eye: 

W ferworze walki szpachlą na preprodzie załadowałem wersję sprzed 2 tygodni, 
zmodyfikowałem zrobiłem commit i uciekłem na aktualny branch iiii? upsi :) 
straciłem odwołanie do commit'a na zmianie którą wykonałem przed chwilą.

No nie ma nigdzie :bug:

Jestem jednym z tych fanatyków którzy lubią dobry interfejs graficzny.

Ja do git'a używam SourceTree i bardzo sobie go cenię, 
ale niestety nie znalazłem funkcji która pozwoliłaby mi odnaleźć moją zmianę.

W tej sytuacji trzeba się "poniżyć" :wink: i użyć konsoli.

Odzyskanie takiego commit'a okazuje się bardzo proste.

1. Odpalasz terminal z poziomu source tree
2. Wpisujesz polecenie **GIT reflog**
3. szukasz swojej zmiany (ps. jak nazywasz swoje commity "Refaktoryzacja" -> good luck with that :grinning:)
4. Wpisujesz polecenie **GIT checkout COMMIT_UID**
5. Ciesz się swoją zmianą - łącz lub łataj :wink: (merge/patch)