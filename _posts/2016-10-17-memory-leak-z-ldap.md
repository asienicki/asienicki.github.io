---
layout: post
title: Memory leak z LDAP
type: post
categories:
- C#
tags:
- Active Directory
- AD
- Dispose
- FindAll
- IDisposable
- LDAP
- Memory Leak
permalink: "/memory-leak-z-ldap/"
image: 'https://sienicki.dev/assets/tiles/memory-leak.jpg'
category: 'blog' 
introduction: Wycieki pamięci od zawsze i z założenia są wredne.
---
Wycieki pamięci od zawsze i z założenia są wredne. 

O ile w małych systemach znalezienie przyczyny wycieku jest proste, szybkie, łatwe i przyjemne, o tyle znalezienie smerfa w czymś większym stanowi już nie lada wyzwanie :mask:

To co mi się przytrafiło to nie wykorzystanie dispose'a w metodzie findAll - tyle. 

Żadnych cudownych opowieści. 
Znalazłem go tylko dlatego, że mogłem testować moduły w swoim kodzie. 
Moduł który najmniej robił najwięcej zjadał... typowe. 

Jeżeli chodzi o te wszystkie analizatory, to nic mi nie było w stanie pomóc, bo wyciek następował poza kodem zarządzanym.

Szybkie demo dla mnie i potomnych jak powinno wyglądać pobranie np. użytkowników z Active Directory:

```csharp
using (var context = new PrincipalContext(ContextType.Domain, "dom.com")) 
{ 
  using (var userPrincipal = new UserPrincipal(context)) 
  { 
    using (var searcher = new PrincipalSearcher(userPrincipal)) 
    { 
      using (var users = searcher.FindAll()) 
      { 
        foreach (var user in users) 
        { 
          using (var de = user.GetUnderlyingObject() as DirectoryEntry) 
          { 
            if (de != null) 
            { 
              Console.WriteLine( de.Properties["givenName"].Value); 
            } 
            
            Console.WriteLine(); 
          } 
        } 
      } 
    } 
  } 
}
```