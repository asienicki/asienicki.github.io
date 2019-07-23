---
layout: post
title: Jak podglądać zapytania Entity Framework
type: post
categories:
- Entity Framework
tags:
- Entity Framework
- linq
- Podglądanie zapytań
- SQL
permalink: "/jak-podgladac-zapytania-entity-framework/"
image: 'https://sienicki.dev/assets/tiles/detective3.jpg'
category: 'blog' 
introduction: ile jest w miarę sensownych sposobów na przechwytywanie zapytań
---
Naszło mnie ostatnio żeby sprawdzić ile jest w miarę sensownych sposobów na przechwytywanie zapytań sql. 
Po wykonaniu szybkiego research'u okazało się, że dowiedziałem się czegoś nowego :wink: Lista poniżej.

1. context.Database.Log = s => Console.WriteLine(s);
2. Glimpse (install-package glimpse.mvc5 glimpse.ef6)
3. IntelliTrace
4. Interceptors w *.config (system.data.entity.Infrastructure.Interception.DatabaseLogger)
5. SQL Profiler
6. ObjectQuery to ToTraceStringvar 
```csharp
var result = from x in appEntities  
         select x; 
var sql = ((System.Data.Objects.ObjectQuery)result).ToTraceString();
```
7. miniprofiler
8. expressprofiler
9. LINQPad
10. The EF Prof Query Profiler UI
11. http://www.huagati.com/L2SProfiler/