---
layout: post
title: Content-Type i GET
type: post
categories:
- ".NET Developer"
tags:
- content-type
- GET
- HTTP GET
- HttpClient
- REST
permalink: "/content-type-i-get/"
image: 'https://sienicki.dev/assets/tiles/waga.png'
category: 'blog' 
introduction: Rozprawka o typie body, którego nie ma, a mogłoby być.
---
Niby nic ciekawego, a jednak postanowiłem się wyżalić :simple_smile:

Podczas jednej ze swoich przygód które nazywam "_**znajdź i zabij potwora**_", znalazłem czające się zło właśnie w content-type.

Co się stało? Bardzo prosty przypadek - aplikacja A i B pytała serwis REST o dane metodą GET. Zapytanie do serwisu wyglądało prawie tak samo.

**A wysyłała w nagłówku Content-Type -> application/json, a B już nie.**

To wystarczyło do zmącenia równowagi wsczechświata w aplikacji B, która powinna dostać taką samą odpowiedź co aplikacja A.

Istotne fakty:

1. REST wystawiony na Sharepoint, który twierdzi, że brak content-type jest nie w porządku (dostałem w odpowiedzi piękny komunikat "bad request - we forgive you"...)
2. Aplikacja B woła serwis przy użyciu metody HttpClient, która nie pozwala na tak perwersyjne tworzenie żądań (przynajmniej ja, nie znalazłem na to sposobu)

I co dalej? No właśnie. Aplikacja B mogłaby użyć innego sposobu komunikowania się z serwisem, ale... dlaczego mam to zmieniać skoro nie do końca wiadomo czyja jest to wina?

I tak rozpoczęło się przeszukiwanie dokumentacji HTTP ::musical_score::

Żeby móc zagłębić się w temat najpierw trzeba się dowiedzieć do czego służy ten HEADER. Można sobie przetłumaczyć Content-Type na PL i wyjdzie nam "typ zawartości"... lub poszperać w [RFC](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.17)i wyjdzie na to samo.. albo nie :stuck_out_tongue:

> The Content-Type entity-header field indicates the media type of the entity-body sent to the recipient or, in the case of the HEAD method, the media type that would have been sent had the request been a GET.

Interesuje nas pierwsza część zdania. W moim tłumaczeniu brzmi to tak:
> Pole nagłówka wskazuje jaki typ mediów znajduje się w ciele wiadomości wysyłanej do odbiorcy.

No właśnie -> w ciele/body wiadomości. A jakie metody HTTP umożliwiają wysłanie ciała? [Kilka](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html), ale na pewno nie GET.

I teraz pytanie za milijon.

Skoro GET nie ma body to po co w HTTP GET ustawiać Content-Type który mówi jakiego typu jest body? Żeby powiedzieć jaki byłby to typ gdyby tam coś było? :confused: