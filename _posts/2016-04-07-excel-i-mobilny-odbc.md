---
layout: post
title: EXCEL i mobilny ODBC
type: post
tags:
- command
- excel
- ODBC
- Query
- XLSX
permalink: "/excel-i-mobilny-odbc/"
image: 'https://sienicki.dev/assets/tiles/kicking-excel-kolor.png'
category: 'blog' 
introduction: Pomysł na urozmaicenie excel'a
---
Dzisiaj chciałem przedstawić pomysł na urozmaicenie nudnego excel'a.

Wyobraźmy sobie, że przychodzi szef i mówi zrób mi na CITO raporty... rzuć wszystko co robisz i zrób mi raporty - od tego zależy przyszłość Twojego zespołu!

Trochę drastycznie, ale mniej więcej tak to może wyglądać.

Czy zamierzam pisać o tym jak pobrać dane z ODBC do excel'a? Jak stworzyć sobie zapytania w SQL? Nie. Nie zamierzam :simple_smile:

Jest fajniejszy temat. Wyobraźcie sobie olbrzymią organizację która ma dużo ważnych dyrektorów. 
Każdy dyrektor ma pod sobą setki pracowników, a jakiś biedny ziomek ma wygenerować oddzielny raport dla każdego z tych dyrektorów dajmy na to z  aktywności umysłowej jego pracowników :simple_smile:  -> Oczywiście aktywności umysłowe są rejestrowane w bazie danych :stuck_out_tongue:

Pani "Małgosia" przygotowała 1 taki raport (wyczesany w kosmos). 
Ktoś dał jej dane w csv z bazy, a ona to przerobiła. 
Teraz trzeba zrobić to dla każdego ważnego dyrektora. Np. 500 raportów. Zrobiła. 
Z tym, że raport był miesięczny.... Upssi! :simple_smile:

Pani Gosia nie chce już tego robić. Oddała zadanie w ręce robotników.

To co może zrobić Pan Kropek? W tym wypadku tylko jedno... odpalić aplikację w mózgu "uprość sobie życie" i czekać na wynik :sleeping:

1 krok - to oczywiście użycie danych z bazy - żeby istniało połączenie z excela do bazy danych (ww. ODBC) i można było sobie te dane w każdej chwili odświeżyć (milijon x lepsze niż csv & św. Copy Paste)

2 krok - rozkminić sposób parametryzacji kwerendy w SQL?!!!!

Może po nazwie pliku? Nie... tragiczny pomysł! Skąd to się bierze? :worried:

To może będę tego excel'a generował... hmmm, ok!

Plik XLSX to przecież nic innego jak ZIP.

Odpalam moje ulubione środowisko i zakładam projekt!

Wrzucam szablon na tempa, żeby mieć śmietnik w 1 miejscu iiii... zaczynam! :smiley:

1. Zmieniam rozszerzenie XLSX na ZIP
2. Rozpakowywuję ZIP
3. Otwieram xml'a connections (chwilę mi zajęło zanim znalazłem odpowiedni xml :simple_smile:)
4. Aktualizuje atrybut command - ustawiam SQL taki jaki chce - dodaje w sekcji WHERE mój parametr
5. Zapisuje XML
6. Pakuje do zip
7. Zmieniam nazwę ZIP na XLSX
8. Testuje... drżą ręce.... działa!

Teraz Pan Kropek może rozpocząć masową produkcję i chwalić się przed ziomkami z pracy, jak to zrobił 500 raportów dla najważniejszych osób w KORPO w kilka minut :smirk:

PS. Pan Kropek zastanawia się jak automatycznie odswieżyć dane w 500 excelach. 
Problem nie doczekał się jeszcze innego rozwiązania niż interfejs białkowy :stuck_out_tongue:

Masz pomysł? Wspomóż Pana Kropka.