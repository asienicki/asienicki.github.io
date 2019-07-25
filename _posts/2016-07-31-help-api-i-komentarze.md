---
layout: post
title: Help Api i komentarze
type: post
categories:
- ASP.NET
tags:
- API
- ASP.NET Web API
- Help Pages
- REST
permalink: "/help-api-i-komentarze/"
image: 'https://sienicki.dev/assets/tiles/helpPages.jpg'
category: 'blog' 
introduction: Help Api - external model
---
Jak dodać do aplikacji asp.net api HELP PAGES, można znaleźć np. na stronie [asp.net](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages).

Co więcej bym chciał do tego dodać? 
- Obsługę kilku providerów. 
Nic więcej - to co jest w dokumentacji odnośnie dokumentacji po prostu działa.

Tylko... co jeśli macie oddzielny projekt na modele do Web API? 
> Ano nic.

W naszej automatycznie wygenerowanej dokumentacji nie będzie żadnych komentarzy z tegoż właśnie projektu.
 Nie dziwota, przecież ładujemy tylko jeden plik xml:
```csharp
config
  .SetDocumentationProvider(
      new XmlDocumentationProvider(
            HttpContext.Current.Server.MapPath("~/App_Data/XmlDocument.xml")));
```
Fajnie by było móc ustawić provider z kilkoma plikami XML. Np. tak:

```csharp
config.SetDocumentationProvider(
  new XmlDocumentationProvider(
    new[] 
    { 
        HttpContext.Current.Server.MapPath("~/TestHelpApi.XML"), 
        HttpContext.Current.Server.MapPath("~/TestHelpApi2.XML")
    }));
```
Wystarczy tylko dodać nowy konstruktor do klasy XmlDocumentationProvider:
```csharp
public XmlDocumentationProvider(IEnumerable documentPaths) 
{ 
  if (documentPaths == null || !documentPaths.Any()) 
  { 
    throw new ArgumentNullException(nameof(documentPaths)); 
  } 
  
  XDocument fullDocument = null; 
  
  foreach (var documentPath in documentPaths)   
  { 
    if (documentPath == null) 
    { 
      throw new ArgumentNullException(nameof(documentPath)); 
    } 

    if (fullDocument == null) 
    { 
      using (var stream = File.OpenRead(documentPath)) 
      { 
        fullDocument = XDocument.Load(stream); 
        }        
    }
    else 
    { 
      using (var stream = File.OpenRead(documentPath)) 
      { 
        var additionalDocument = XDocument.Load(stream); 

        fullDocument.Root?.XPathSelectElement("/doc/members")
                          .Add(additionalDocument.Root?
                                    .XPathSelectElement("/doc/members").Elements()); 
        } 
    } 
    } 
    _documentNavigator = fullDocument?.CreateNavigator(); 
}
```
Powyższy kawałek kodu znalazłem na [stacku](http://stackoverflow.com/a/33765666).
Po zastosowaniu możesz cieszyć się REST API z komentarzami z osobnej assembly.