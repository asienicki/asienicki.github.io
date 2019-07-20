---
layout: post
title: Data jako liczba - UNIX
type: post
categories:
- C#
tags:
- Date
- DateTime
- UNIX
permalink: "/data-jako-liczba-unix/"
image: 'https://sienicki.dev/assets/tiles/tux-Time.png'
category: 'blog' 
introduction: Dzisiaj znów wypłynął temat magicznej daty zapisanej jako liczba w formacie "1446073200000".
---
Dzisiaj znów wypłynął temat magicznej daty zapisanej jako liczba w formacie "1446073200000".

Jak to zapisać w C#? Poniżej przykład:

```csharp
public static DateTime UnixTimeStampToDateTime(this int unixTimeStamp) 
{ 
    var dtDateTime = new DateTime(1970, 1, 1, 0, 0, 0, 0, DateTimeKind.Utc); 
    dtDateTime = dtDateTime
                  .AddSeconds(unixTimeStamp)
                  .ToLocalTime(); 
    
    return dtDateTime; 
}
```
Natomiast jeżeli chodzi o szybką konwersję w javascript, zapisu "**Date(1446073200000)**" to wygląda to tak:

```javascript
dt = dt.replace("/Date(", ""); 
dt = dt.replace(")/", ""); 
var dateValue = new Date(parseInt(dt, 10));
```
Gorące pozdrowienia dla Mariusza za zainspirowanie notki :smile: