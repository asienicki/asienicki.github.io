---
layout: post
title: Operator koalescencyjny
type: post
categories:
- C#
tags:
- "??"
- C#
- koalescencja
- null-coalescing
- null-coalescing operator
- Operator ??
- Operator koalescencyjny
- operator zjednoczenia
- operator łączenia
- zjednoczenie
- złączenie
- łączenie
permalink: "/operator-koalescencyjny/"
image: 'https://sienicki.dev/assets/tiles/zapytanie.jpg'
category: 'blog' 
introduction: Nullowy operator koalescencyjny
---
* * *
__O patrz tutaj... za dużo tu kodu.. można to napewno uprościć np. w tym miejscu możesz użyć operatora koalescencyjnego...__ :joy:
* * *
No właśnie. Wczoraj pierwszy raz spotkałem się z tym pojęciem w książce i myślę... że tam też zostanie. Wróci może w łaski jak będzie trzeba zadać jakieś dziwne pytanie na rozmowie kwalifikacyjnej, tylko trzeba się jeszcze nauczyć to wymawiać :grinning:

O co chodzi? Jak poszukacie, to dowiecie się jaką książkę mam aktualnie na tapecie.

**Nullowy operator koalescencyjny** (null-coalescing operator) - to nic innego jak podwójny znak zapytania :tongue:

```csharp
int result = GetNullableIntValue() ?? RandStatic.Zero;
```

Jeśli metoda **GetNullableIntValue** zwróci null to do zmiennej result zostanie przypisana wartość RandStatic.Zero w przeciwnym wypadku przypisana zostanie zwrócona wartość.

Spróbujcie wymówić to szybciej :smiling_imp:

Google mówi, że coalescing to tyle co łączyć/zjednoczyć, ale koalescencja brzmi bardziej profesjonalnie i Pan Grabik bardzo dobrze o tym wiedział :simple_smile:

A może by tak użyć... **OPERATORA ŁĄCZENIA?**

Pfff...! takie coś każdy może wymówić :wink:
Link do źródła: [http://msdn.microsoft.com/pl-pl/library/ms173224.aspx](http://msdn.microsoft.com/pl-pl/library/ms173224.aspx)
PS. Sprawdziłem również biblię wg Troelsen'a - używa on tam po prostu nazwy "**operator ??**".

_"Koalescencja – w fizykochemii koloidów jest to proces, w którym dwie lub więcej cząstek łączy się ze sobą, tworząc pojedynczą cząstkę._  
_Koalescencja zachodzi w układach koloidalnych (np. pianach i emulsjach). Polega wówczas na łączeniu się cząstek fazy rozproszonej (kropli cieczy lub pęcherzyków gazu) w większe, zmniejszając stopień dyspersji układu. Doprowadza to do rozbicia emulsji na odrębne fazy lub opadnięcia piany._  
_Proces koalescencji, zachodzący w chmurach, prowadzi do powstawania kropli deszczowych."_

[~Paulo Coehlo](http://pl.wikipedia.org/wiki/Koalescencja)