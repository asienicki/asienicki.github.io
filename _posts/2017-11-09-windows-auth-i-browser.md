---
layout: post
title: Windows Auth i browser
type: post
categories:
- ASP.NET
- IIS
tags:
- credentials
- kerberos
- Windows Authentication
permalink: "/windows-auth-i-browser/"
image: 'https://sienicki.dev/assets/tiles/doorman.jpg'
category: 'blog' 
introduction: Konfiguracja Windows Authentication.
---
Dzisiaj omówię jak prawidłowo skonfigurować **uwierzytelnianie windows** 
aplikacji intranetowej hostowanej na IIS.

Jaki może być z tym problem? 
To już zależy co chcemy osiągnąć :smile:

Wszyscy wiemy, że z IIS'a należy wybrać site->authentication i włączyć "Windows Authentication". 
Cała reszta wedle upodobań.

No dobra, ale hostuje aplikacje na IIS w domenie **pluszowy.mis** 
dostępną pod adresem **http://superapka.pluszowy.mis** 
(nawet mam skonfigurowane AD CS i certyfikat dla serwera **mojiis.superapka.pluszowy.mis** 
i nazwy dns: **http://superapka.pluszowy.mis**) iii... 
na stacji roboczej **koralgol.pluszowy.mis** 
(również podpiętej do domeny **pluszowy.mis** )
uruchamiam przeglądarkę i wpisuje adres **http://superapka.pluszowy.mis&nbsp; ->** tadadada :loudspeaker:

Czy to Internet Explorer czy Chrome - zostanę poproszony o podanie swoich danych do logowania... 
ok... ale po co? 

Przecież jestem w domenie, mój system wie kim jestem! :open_mouth:

Zanim przejdę dalej - dodam tylko, że strona intranetowa **http://superapka.pluszowy.mis** 
nie potrzebuje się bawić w delegację uprawnień na wiele "hopów", 
więc poniższy krok wykonuje z pełną świadomością tego co robię :alien:

W miejscu w którym skończyliśmy poprzedni krok w IIS
 - przechodzimy do "providers" dla "Windows Authentication". 
 Na liście domyślnie znajduje się Negotiate i NTLM.

Super! To teraz wystarczy, że NTLM będzie na liście pierwszy. 
Restart site-puli-IIS'a-7xctrl+shift+F5 mambojumbo.... 
odpalam **http://superapka.pluszowy.mis** w IE i dalej żąda poświadczeń :expressionless:

Dzieje się tak ponieważ z jakiegoś magicznego powodu 
moja stacja robocza **koralgol.pluszowy.mis** nie rozpoznaje 
strony **http://superapka.pluszowy.mis** jako strony intranetowej w domenie... niah.

Na taką ewentualność jest prosty trik 
- trzeba mieć chyba nawet administratora żeby to zrobić (ale to tak na 50%) :dizzy_face:

IE->settings->Internet Options->Security-> wybieramy "**Local intranet**" 
-> Sites -> 
(pewnie masz wszystko zaznaczone - a nawet jak nie masz i zaznaczysz to nie rozwiąże to Twojego problemu) 
-> advanced-> i teraz wpisujemy adres bez protokołu "**superapka.pluszowy.mis**" 
-> add -> close -> ok ->ok-> save 
-> poślinić palec -> F5

Teraz... w IE coś się dzieje. 
Strona **http://superapka.pluszowy.mis** została załadowana bez pytania o poświadczenia windows. 
Yupi! Windows sam sobie poradził - zuch! :mask:

No dobrze. To teraz pragnąc dokonać eskalacji sukcesu. 
Odpalam stronę **http://superapka.pluszowy.mis**  w przeglądarce gogiela Chrome'a.

I co? Ku mojemu zdziwieniu (mam nadzieję, że waszemu też) okazuje się, 
że Chrome dalej prosi o hasło do Windows'a...

Kończąc tą wydłużającą się narrację przygody rodem z komedii kryminalnej 
- wystarczy, że wrócimy do IIS'a i z listy " **Providers**" 
tam gdzie jest NTLM i Negotiate - usuniemy "**Negotiate**".

Tak! Pewnie to zrobiłeś! 

PoolRecycle, Website restart i IIS reset - i to z konsoli, 
restart chrome - nowa zakładka incognito 
- ślinimy palec i wprowadzamy jeszcze raz adres: **http://superapka.pluszowy.mis&nbsp;** 
tylko tym razem nie kopiując, a pisząc to samodzielnie 
- liczymy na magiczną moc wstukanych klawiszy... strona się ładuje. 

Chrome nie prosi o dane windows'a - sam przekazuje uprawnienia.

**Podsumowując** i wyciągając z mojej zabawy narracyjnej **mięsko**:

1. "Windows Authentication" - enabled
2. "Windows Authentication"-> providers - tylko NTLM
3. IE->settings->Internet Options->Security-> 
wybieramy " **Local intranet**" 
-> Sites -> advanced-> " **superapka.pluszowy.mis**" -> add -> close -> ok/save

Na koniec ciekawostka... z racji tego, że lubię Kerberosa chociaż niezbadane są ścieżki jego ticketów... okazało się, że przy Providers ustawionym na Negotiate aplikacja nie pyta o dane logowanie kiedy dostaje się do niej po adresie **fakemis.dnstypua**...
Czyżby dostawanie się do aplikacji po pełnym adresie będącym jednocześnie prawdziwym pełnym adresem serwera nie było dobrym pomysłem?

Do testów wykorzystałem:
- Windows 10
- Windows Server 2016

**Rerozkmince** towarzyszyły rytmy "[Best of ELECTRO SWING Mix September 2017](http://www.youtube.com/watch?v=RPMTZfNSXYQ)" - przyjemnie i pozytywnie :headphones: :wink: