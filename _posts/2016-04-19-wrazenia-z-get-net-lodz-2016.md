---
layout: post
title: Wrażenia z GET.NET
type: post
categories:
- ".NET Developer"
tags:
- '2016'
- Akka.NET
- devOps
- GET.NET
- IoT
- Łódź
permalink: "/wrazenia-z-get-net-lodz-2016/"
image: 'https://sienicki.dev/assets/tiles/relation.jpg'
category: 'blog' 
introduction: Notatki z konferencji
---
Hej!

Niedawno wróciłem z prawie 3 dniowego wypadu do Łodzi którego celem kulminacyjnym była 
konferencja GET.NET zorganizowana na UŁ Wydziału Filologicznego.

Wyjechaliśmy z Warszawy w piątek po pracy. Podróż trwała krótko, bo niecałe 2 godziny. 
Jechaliśmy A2 i o dziwo zapłaciliśmy trochę ponad złotówkę - trzeba pamiętać żeby zjechać zjazdem na Zgierz :simple_smile:

Po dotarciu do hotelu okazało się, że plotka o trzęsących się budynkach przy przejeździe tramwaju - to wcale nie plotka :simple_smile:

Sama konferencja - od strony organizacyjnej:
- przyjazne 4 ściany
- każdy uczestnik miał swobodny dostęp do napojów (kawa/herbata/woda)
- ponadto mega słodkie ciastka
- obiad w 3 różnych wariantach
- welcome pack z notesem, długopisem, ankietą,  linką do karty oraz naklejki na laptopa :stuck_out_tongue_winking_eye:

Od strony merytorycznej jestem zachwycony :simple_smile:

Tam gdzie była możliwość wybrania prelekcji czułem, że podjąłem słuszną decyzję. 
Po wszystkim myślę, że to była jedna z lepszych konferencji w jakich miałem przyjemność brać udział - oby takich więcej!

To może podsumowanie z każdej prelekcji na której byłem.

"**FROM ZERO TO HERO WITH RUNNING YOUR ASP.NET 5 APPLICATION IN A [DOCKER](http://www.docker.com/) CONTAINER**" - [Maurice de Beijer](http://blogs.msmvps.com/theproblemsolver/)

Szczerze to wg mnie najsłabsza prezentacja - nie dlatego, że była słaba - bo nie była, ale dlatego, że zanim mi się spodobał temat - prezentujący mnie do niego zniechęcił tuż po tym jak powiedział, że w Windows działa to kiepsko, ale nad tym pracują... jakoś tak ostatnio nie ufam rozwiązaniom które jeszcze nie działają produkcyjnie - startupów jest wiele, a ile z tego wychodzi to każdy potrafi ocenić sam. A pomysł samego [Docker'a](http://www.docker.com/)? Bomba. Mamy paczkę/aplikację np.  ASP.NET Core rc2 (też beta :P) którą możemy zlinkować z naszą aplikacją napisaną np. w ASP.NET Core 1. Linkujemy 1 do 2 i działa... uwaga - na różnych środowiskach (Linux/Windows). Mamy paczkę... i możemy zrobić **build/ship/run**  gdzie tylko chcemy. Żeby cokolwiek zrobić w [docker](http://www.docker.com/) trzeba się posługiwać jego własną konsolą - alternatywą jest powershell.

Zajawki:

- [Rancher 1.0](http://rancher.com/announcing-rancher-1-0-ga/) - jest to kontener który zarządza kontenerami :)
- [Kibana](http://www.elastic.co/products/kibana) -  do wizualizacji danych
- [ElasticSearch](http://www.elastic.co/) - silnik wyszukiwania

Slogany:

- Takes care: move system to other place.
- It's gonna be the future soon!

"**CO TO JEST INTERNET OF THINGS I CO MA DO TEGO .NET**" -[ Arkadiusz Benedykt](http://www.benedykt.net/)

Prezentacja super - nie interesowałem się ostatnio IoT i wysłuchanie prelekcji Arka wzbudziło we mnie wyrzuty sumienia.
Zaczęło się standardowo - opowiadanie o historii zabawy z "mikro" procesorami :wink:
Dużo się zmieniło... nie na przestrzeni 20 lat, bo wiadomo, ale np. od mojej ostatniej zabawy z arduino (zraziłem się bo C i płytkę spaliłem :wink:).

Mnogość dostępnych płytek jest rzeczywiście teraz duża, np.:

- netduino - .NET
- [raspberry pi](http://www.raspberrypi.org/)
- Spino - javascript
- microPython - python

Dosyć ciekawe jest to, że wcale nie muszę męczyć się w C lub w czymś podobnym, żeby napisać program na płytkę - mogę pisać w ograniczonym c# w Visual Studio. W ulubionym języku i środowisku :wink:

Słyszałem o [raspberry pi](http://www.raspberrypi.org/), ale nigdy nie zgłębiałem tematu.

Na takiej małej płytce można zainstalować np. Windows 10 IoT i domyślnie wgrać program który np. miga żarówką - na prezentacji mieliśmy demo na żywo - zadziałało za 1 razem i to podwoiło efekt mojego wow. 
Co jest takiego wow?  Woow jest to, że aplikacja zadziałała natychmiast. Zmodyfikował częstotliwość świecenia - od razu zaczęło migać wolniej/szybciej. Na arduino jak się bawiłem to problemów przy starcie było sporo. Tutaj? Plug & Play.

Nie obyło się bez wątku chmury... koszt utrzymania połączenia urządzenia na azure to około 50 euro, ale za to macie... :simple_smile:

Zajawki:

- [jawbone](http://jawbone.com/)
- [internet of shit](http://twitter.com/internetofshit)
- [if this than that - IFTTT](http://ifttt.com/)
- windows IoT remote client (połączył się zdalnie z [raspberry pi](http://www.raspberrypi.org/) ze swojego komputera)
- big data & machine learning

"**IS DEVOPS IN BANKING POSSIBLE**" - [Piotr Stapp](http://stapp.space/)
Popularny temat - każdy chce być przecież dev ops... no bo przecież jesteśmy programistami, żeby programować, a nie marnować czas na bzdury :simple_smile:
Piotr poprowadził prezentację ciekawie. Zajawki, krótkie dema plus opowieści. Tak jak to sobie wyobrażałem :simple_smile:

Zajawki/tipy:

- agregacja logów ([Splunk](http://www.splunk.com/), [ELK](http://www.elastic.co/webinars/introduction-elk-stack), [gray log](http://www.graylog.org/),  [zestawienie](http://stackshare.io/log-management), [zestawienie 2](http://blog.profitbricks.com/top-47-log-management-tools/))
- automatyzacja deploy'a ([Jenkis](http://jenkins.io/)+ powershell, [chef](http://www.chef.io/chef/), [puppet](http://puppet.com/product/how-it-works), [ansible](http://www.ansible.com/application-deployment), [Octopus Deploy](http://octopus.com/))
- service discovery ([consul](http://www.consul.io/), [apache zookeeper](http://zookeeper.apache.org/), [etcd](http://coreos.com/etcd/docs/latest/), [netflix eureka](http://github.com/Netflix/eureka/wiki))
- [release manager](http://blogs.msdn.microsoft.com/alming/2015/01/19/using-web-config-transformations-and-release-manager/) ([++](http://www.colinsalmcorner.com/post/config-per-environment-vs-tokenization-in-release-management))
- wersjonowanie bazy danych (VS DB project, commercial stuff, [DbUp](http://dbup.github.io/))

"**FROM LEGACY TO DDD**" -  [Andrzej Krzywda](http://andrzejonsoftware.blogspot.com/)

Hit. Wszystkie miejsca w mniejszej sali zostały zajęte (+schody :simple_smile:). Widać, że programowanie funkcyjne cieszy się dużo mniejszą popularnością niż DDD :wink:

Sukcesem jednak moim zdaniem był temat, marketing zrobił swoje.
 Każdy był w końcu ciekawy jak swojego wielkiego monolita przekształcić do DDD. 
 Poruszony został temat Event Sourcing i CQRS. Złotego środka niestety nie ma. Krok po kroku.

Na koniec rzucone zostało przewrotne hasło:

"**_A co jeśli CQRS jest przystankiem w podróży do programowania funkcyjnego_?**" :stuck_out_tongue_winking_eye:

Krótkie notatki:

- prawo Godwina:  _Wraz z trwaniem dyskusji w Internecie, prawdopodobieństwo użycia porównania, w którym występuje nazizm bądź Hitler, dąży do 1_
- [hamming distance](http://exercism.io/) (Download and solve practice problems)
- [React](http://reactjs.net/)(fb & instagram) - Andrzej bardzo w to wierzy
- [Redux](http://github.com/gaearon/redux-devtools) (reload & apply)

* * *

"**TWORZENIE SYSTEMÓW ROZPROSZONYCH Z WYKORZYSTANIEM AKKA.NET**" - [Michał Jasiorowski](http://www.goldenline.pl/michal-jasiorowski/)

Zdecydowanie najlepsza według mnie prezentacja dnia. Dlaczego? Lubię ten styl. 
Wymyślenie celu i zrealizowanie go w zwykły sposób, a potem demo jak zrobić to lepiej przy użyciu narzędzia które mam prezentować.

Co nam daje Akka.NET?

- thread safe
- trwałość
- pracę stanową
- prosty/bezpieczny/przejrzysty/realizujący skomplikowane działania kod
- wysoką wydajność (tutaj liczby były trochę wyidealizowane)
- łatwą rozszerzalność
- prostą skalowalność

Luźne notatki:

- Klastrowanie w AKKA jest zapożyczone z kasandry
- większość aplikacji to 20% zapisu i 80 % odczytu (Pareto?:))
- AKKA.Remoting & AKKA cluster
- SQL Server & single point of failure
- [akka bootcamp](http://github.com/petabridge/akka-bootcamp)

"**DESIGNING API FOR MOBILE APPS**" - [Wojciech Erbetowski](http://erbetowski.pl/)
Prezentacja była dobra. Wojtek opowiadał o typowych problemach pracy z aplikacjami mobilnymi. Posługiwał się liczbami i opowieściami z życia. Przyjemne do posłuchania na koniec konferencji/dnia :simple_smile:

Przytoczę jedno z najważniejszym zdań tej prelekcji:

_Nie wszystko co jest dobrym REST Api jest najlepszym dla aplikacji mobilnej_

Zajawki:

- [HATEOAS](http://en.wikipedia.org/wiki/HATEOAS)
- [Swagger](http://swagger.io/)

Zostaliśmy w Łodzi jeszcze niedzielę. Wbrew szeroko panującej opinii "_co tu robić_" znalazło się kilka fajnych zajęć :wink: 
Odwiedziliśmy muzeum kinematografii i Arboretum w Rogowie.

Niczego nie żałuję, polecam :simple_smile: