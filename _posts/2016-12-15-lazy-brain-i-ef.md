---
layout: post
title: Lazy brain i EF
type: post
categories:
- ".NET Developer"
- C#
- Entity Framework
- ORM
tags:
- "#BrainFuck"
- Entity Framework
- Lazy Loading
- materializacja
permalink: "/lazy-brain-i-ef/"
image: 'https://sienicki.dev/assets/tiles/question.png'
category: 'blog' 
introduction: Dzisiejszy post jest zadedykowany lazy loadingu i brain fucku jaki mieli koledzy 5 minut przed końcem prac deweloperskich.
---
Hello!

Dzisiejszy post jest zadedykowany lazy loadingu i brain fucku jaki mieli koledzy 5 minut przed końcem prac deweloperskich :wink:

Technicznie i żołniersko.

- Projekt: .NET
- ORM: EF
- Moc: Lazy Loading
- Problem: Lazy Things

To może na początek kawałek kodu:

```csharp
using (var db = new ContosoTestContext()) 
{ 
  var category = db.Categories.First(x => x.Articles.Any()); 
  
  if (category.Articles.Any())
  {
  } 
}
```
Piękny, czysty i jakże podręcznikowo-szkolny przykład.

Ile zapytań poleci do bazy danych?
Ile rekordów dostanie aplikacja?

Paczasz? I co? 2 razy? Jakby było 2 razy to by nie pisał posta :wink:

A no piszę! Bo właśnie do bazy idą 2 query... ale za to jakie :cold_sweat:

W pierwszej linijce pobieram z bazy jeden rekord kategorii. 
A w drugiej linijce jeżeli nic nie zmienialiście w waszym kontekście bazy danych z lazy loading, bo domyślnie wartość Configuration.LazyLoadingEnabled jest ustawiona na true - to... zostaną pobrane wszystkie artykuły z bazy danych do pamięci, a dopiero potem zostanie wykonane polecenie Any.

Chyba nie muszę pisać, jak bardzo opłakane w skutkach jest użycie powyższego kodu... brr!

Żeby nie być gołosłownym to 1 query wygląda tak:

```sql
SELECT TOP (1) 
    [Extent1].[Id] AS [Id], 
    [Extent1].[Name] AS [Name] 
FROM [dbo].[Categories] AS [Extent1] 
WHERE EXISTS (
              SELECT 1 AS [C1] 
              FROM [dbo].[Articles] AS [Extent2] 
              WHERE [Extent1].[Id] = [Extent2].[CategoryId])
```

a 2 query w ten sposób:

```sql
SELECT 
    [Extent1].[Id] AS [Id], 
    [Extent1].[Name] AS [Name], 
    [Extent1].[CategoryId] AS [CategoryId] 
FROM [dbo].[Articles] AS [Extent1] 
WHERE [Extent1].[CategoryId] = @EntityKeyValue1
```
Tak więc, pierwsze zapytanie zwróci faktycznie jeden rekord z bazy, ale za to drugie zwróci **n** rekordów.

Dlaczego? 
Lazy loading - czyli leniwe ładowanie, pobiera tylko te dane które są nam aktualnie potrzebne, a my przez napisanie **category.Articles** wywołujemy geciora, który z kolei wywołuje groźnego Wielkiego Pana i Mistrza Materializatora!!! :imp:

Jak się przed tym uchronić? Uważać... :neckbeard: 

Na przykład przy składniach tego typu używać bardziej kontekstu, 
niż kontekstu który jest powiązany z obiektem, bo tak jak wyżej - można się zdziwić.

To może teraz poprawne użycie Any:

```csharp
using (var db = new ContosoTestContext()) 
{ 
  var category = db.Categories.First(x => x.Articles.Any()); 
  
  if (db.Articles.Any(x=>x.CategoryId== category.Id)) 
  {     
  } 
}
```
I to wyśle 2 zapytania i zwróci 2 rekordy.

Query 1:

```sql
SELECT TOP (1) 
  [Extent1].[Id] AS [Id], 
  [Extent1].[Name] AS [Name] 
FROM [dbo].[Categories] AS [Extent1] 
WHERE EXISTS (
      SELECT 1 AS [C1] 
      FROM [dbo].[Articles] AS [Extent2] 
      WHERE [Extent1].[Id] = [Extent2].[CategoryId] )
```

Query 2:

```sql
SELECT 
  CASE 
    WHEN (
            EXISTS(SELECT 1 AS [C1] 
            FROM [dbo].[Articles] AS [Extent1] 
            WHERE [Extent1].[CategoryId] = 2 )
          ) 
    THEN cast(1 as bit) 
    ELSE cast(0 as bit) 
  END AS [C1] 
FROM (SELECT 1 AS X) AS [SingleRowTable1]
```

Bardzo dziękuję wszystkim za uwagę.
Ciekawe ile razy wy złapaliście się na takim brain fuck'u :grin: