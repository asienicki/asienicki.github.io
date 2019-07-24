---
layout: post
title: Web.Config Debug & Release
categories:
- C#
tags:
- configuration
- release
- Replace
- transform
- web.config
- web.debug
- web.release
permalink: "/web-config-debug-release/"
image: 'https://sienicki.dev/assets/tiles/konfiguracja.png'
category: 'blog' 
introduction: Transformacja
---
Dziś zmuszony silną potrzebą, musiałem nauczyć się jak zrobić żeby po publishu mieć inny web.config od tego na którym pracuje lokalnie. 
Np. żeby mieć inny connectionstring, albo inny system.serviceModel wszystko można załatwić za pomocą 1 linijki kodu... (jak to zwykle bywa).

Musimy mieć 3 web.configi, 
jeden główny web.config jeśli da się go rozwinąć to pewnie są 2 pozostałe, 
jeśli nie to trzeba kliknąć go prawym i wybrać "Add Config Transform" tadaa..! się wygenerował.

Z web.config skopiowałem wszystko do debug.config, a do web.release config dodałem przysłowiową linijkę kodu (dokładnie 3:P) więc,  
w konfiguracji dodajemy atrybut, wygląda to tak:

```xml
<configuration xmlns:xdt="http://schemas.microsoft.com/XML-Document-Transform"></configuration>
```
a w sekcjach które chcemy zamienić dajemy np.

```xml
<connectionstrings xdt:transform="Replace">
...
<system.servicemodel xdt:transform="Replace">
...</system.servicemodel>
</connectionstrings>
```

To jest to... koniec roboty. VS po zrobieniu publisha (zależnie od trybu debug/release, zrobi merge dwóch web.configów, web.config oraz web.X.config :simple_smile: