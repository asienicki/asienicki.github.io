---
layout: post
title: Config i transformacja
type: post
categories:
- ASP.NET
tags:
- config
- debugging
- transofrmacja
- web.config
- web.debug
- web.release
permalink: "/config-i-transformacja/"
image: 'https://sienicki.dev/assets/tiles/konfiguracja.png'
category: 'blog' 
introduction: Live transformation.
---
Pisałem już o transformacji konfiguracji pliku web.config [wcześniej](https://sienicki.dev/web-config-debug-release/)
Możliwość tworzenia własnej konfiguracji zależnie od środowiska na którym chcemy opublikować aplikację jest po prostu bezcenna.

No właśnie - transformacje dla aplikacji WWW działają tylko przy publikowaniu treści na serwer. A jeżeli chcemy pójść dalej?

Mam transformacje i np. 2 środowiska testowe, a chciałbym na żywo móc przełączać się pomiędzy moimi konfiguracjami testowymi.

Tak po prostu nie można.

Niestety trzeba poświęcić na to chwilę czasu, ale zysk jest oczywisty :simple_smile:

Post jest inspirowany [postem z bloga Vidar's musings](http://www.kongsli.net/2012/01/13/enabling-web-transforms-when-debugging-asp-net-apps/).

Przepis na korzystanie z transformacji podczas debugowania:

[1] Dodaj do swojego *.csproj zaraz za poniższą linią:

```xml
<Content Include="Web.config" />
```
linię:

```xml
<None Include="Web.template.config" />
```
Następnie na końcu pliku dodaj property group i target:

```xml
<PropertyGroup>
  <BuildDependsOn>CustomWebConfigTransform; $(BuildDependsOn);</BuildDependsOn>
</PropertyGroup>
<Target Name="CustomWebConfigTransform">
  <TransformXml source="Web.template.config" transform="Web.$(Configuration).config" destination="Web.config" />
</Target>
```
[2] Dodaj plik do projektu web.template.config

[3] Usuń z repozytorium kodu plik Web.config - od teraz jego zawartość będzie zmieniała się dynamicznie - Twoim nowym mentalnym Web.config jest Web.template.config

[4] Ciesz się debugowaniem ze zmienną konfiguracją :relaxed: