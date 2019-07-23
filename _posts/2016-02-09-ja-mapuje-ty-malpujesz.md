---
layout: post
title: Ja mapuje, ty małpujesz
type: post
categories:
- C#
tags:
- AutoMapper
- expressmapper
- mapowanie
- mapster
- oomaper
- porównanie mapperów
- tinymapper
- valueinjecter
permalink: "/ja-mapuje-ty-malpujesz/"
image: 'https://sienicki.dev/assets/tiles/monkey.jpg'
category: 'blog' 
introduction: Porównanie mapperów
---
Ile to już lat minęło od pierwszego projektu w którym używałeś mapper'a? To był AutoMapper, prawda? :wink:

Najlepszy przyjaciel programistów, wróg wydajności. 
I ta rozkminka za każdym razem... czy  ja na pewno mogę mapować (nie muszę małpować). 
Dosyć dawno już Mariusz polecał ValueInjecter, ale różnica wydajnościowa była nieznaczna i ciężko było znaleźć coś lepszego. 
Na szczęście od tamtej rozmowy minęło już trochę czasu, a na rynku pojawiło się więcej bibliotek mapujących :simple_smile:

Na stronie [ExpressMapper'a](http://www.expressmapper.org/#benchmarks "ExpressMapper")można sobie porównać wydajność poszczególnych **_małpek_**. Zmałpuje i przetworzę wyniki testów w postaci rankingów.

## Ranking

Osie X - liczba zmapowanych obiektów  
Osie Y - czas mapowania podany w milisekundach  
wartość -1 - niewspierany rodzaj mapowania

### XXL - Zaawansowane mapowanie - zagnieżdzone kolekcje - afterMap - beforeMap

[![porownanie-mapperow]({{ site.baseurl }}/assets/images/porownanie-mapperow.png "porównanie mapperów XXL")](https://sienicki.dev/assets/images/porownanie-mapperow.png)

1. ExpressMapper  
2. MAŁPA\*  
3. Mapster  
4. AutoMapper  
5. ValueInjecter

### XL - Zaawansowane mapowanie - zagnieżdzone kolekcje

[![porownanie-mapperow xl]({{ site.baseurl }}/assets/images/porownanie-mapperow-xl.png "porównanie mapperów XL")](https://sienicki.dev/assets/images/porownanie-mapperow-xl.png)

1. ExpressMapper  
2. MAŁPA  
3. OoMapper  
4. Mapster  
5. ValueInjecter  
6. AutoMapper

### L - relacja 1 do wielu

[![porownanie-mapperow l]({{ site.baseurl }}/assets/images/porownanie-mapperow-l.png "porównanie mapperów L")](https://sienicki.dev/assets/images/porownanie-mapperow-l.png)

1. ExpressMapper
2. MAŁPA
3. OoMapper
4. Mappster
5. ValueInjecter
6. AutoMapper

### M - relacja 1 do 1

[![porownanie-mapperow m]({{ site.baseurl }}/assets/images/porownanie-mapperow-m.png "porównanie maperów M")](https://sienicki.dev/assets/images/porownanie-mapperow-m.png)

1. MAŁPA
2. ExpressMapper
3. OoMapper
4. Mapster
5. ValueInjecter
6. AutoMapper

### S - podstawowe mapowanie

[![porownanie-mapperow s]({{ site.baseurl }}/assets/images/porownanie-mapperow-s.png "porównanie maperów S")](https://sienicki.dev/assets/images/porownanie-mapperow-s.png)

1. MAŁPA
2. TinyMapper
3. ExpressMapper
4. OoMapper
5. Mapster
6. ValueInjecter
7. AutoMapper

### XS - proste mapowanie enum'ów

[![porownanie-mapperow xs]({{ site.baseurl }}/assets/images/porownanie-mapperow-xs.png "porównanie mapperów XS")](https://sienicki.dev/assets/images/porownanie-mapperow-xs.png)

1. MAŁPA
2. ExpressMapper
3. ValueInjecter
4. AutoMapper

Ok.. nie wiem jak was, ale mnie to trochę zaintrygowało, przebitka imponuje. :simple_smile: 
Zarówno pod względem wydajności jak i możliwości na korzyść ExpressMapper'a. 
Zobaczymy jak wyjdzie w praniu, w końcu _nie ma znaczenia jak głosują, ale kto głosy liczy_ :wink: dla niedowiarków ekipa ExpressMapper'a przygotowała [projekt z testami](http://github.com/expressmapper/ExpressMapper/tree/master/Benchmarks) żeby samemu można było sobie potestować.

Zostało tylko podsumować wszystkie testy. Im więcej punktów tym gorzej. 
Liby które w teście nie dawały rady dostały karne 1000 punktów.

| **lib\test** | **XXL** | **XL** | **L** | **M** | **S** | **XS** | **suma pkt** |
| MAŁPA | 2 | 2 | 2 | 1 | 1 | 1 | 9 |
| ExpressMapper | 1 | 1 | 1 | 2 | 3 | 2 | 10 |
| ValueInjecter | 5 | 5 | 5 | 5 | 6 | 3 | 29 |
| AutoMapper | 4 | 6 | 6 | 6 | 7 | 4 | 33 |
| Mapster | 3 | 4 | 4 | 4 | 5 | 1000 | 1020 |
| OoMapper | 1000 | 3 | 3 | 3 | 4 | 1000 | 2013 |
| TinyMapper | 1000 | 1000 | 1000 | 1000 | 2 | 1000 | 5002 |

I tak mistrzem małpowania jest **MAŁPA**!!! #hura #jupi #zaskoczenie

Vice mistrzem jest **ExpressMapper** przegrywając z MAŁPĄ tylko 1 punktem, a daleko w tyle za czołówką na 3 miejscu mamy **ValueInject**'era.

Natomiast 4 miejsce zajmuje AutoMapper - jako relikt przeszłości - na zawsze zostanie w naszych sercach :heart:

A pozostałe liby... no cóż, przegrały przez karniaki.

### Rys historyczny ExpressMapper'a

Po krótkich poszukiwaniach znalazłem na GitHub'ie [inicjalny commit - 22 IV 2015](http://github.com/expressmapper/ExpressMapper/commit/23b758107c56b2e7f67f69ec4f970df2c405e5a5 "ExpressMapper"). Pierwsza paczka do [NUGET](http://www.nuget.org/packages/Expressmapper)została wgrana 16 V 2015 -[wersja 0.9.0](http://www.nuget.org/packages/Expressmapper/0.9.0)

W porównaniu do AutoMapper - po niecałym roku popularność jest prawie zerowa.

Liczby pobrań:  
AutoMapper - 3 110 477  
ExpressMapper - 4 607

Pozostaje nam jedynie trzymać kciuki za rozwój :wink:
_MAŁPA* -  to tyle co mapowanie ręczne._

