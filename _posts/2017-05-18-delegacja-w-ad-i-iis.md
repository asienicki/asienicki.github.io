---
layout: post
title: Delegacja w AD i IIS
type: post
permalink: "/delegacja-w-ad-i-iis/"
image: 'https://sienicki.dev/assets/tiles/darkmaster.png'
category: 'blog' 
introduction: Dziś zamierzam opisać proces delegacji pomiędzy witrynami na różnych serwerach w kontekście jednej domeny.
---
Dziś zamierzam opisać proces delegacji pomiędzy witrynami na różnych serwerach w kontekście jednej domeny.

Po co? 

Generalnie wymyśliłem sobie przypadek w którym mam aplikację numer 1 i aplikację numer 2. 

Authentication w obu aplikacjach jest ustawione na "Windows Authentication". 
W app1 wyświetlam aktualnego użytkownika używając:

@User.Identity.Name w widoku MVC.

App2 będzie zwracać poprzez web api również aktualnego użytkownika. Całą sytuację możemy zasymulować poprzez kod zaprezentowany poniżej. Zwykły kontroler:

```csharp
public class HomeController : Controller
{
  public string Index()
  {
      return User.Identity.Name;
  }

public string Go(string id)  
{  
    var client = new RestClient($"http://{id}");  
    var request = new RestRequest("api/values", Method.GET)  
      {  
          UseDefaultCredentials = true  
      };

    var response = client.Execute(request);

    var content = response.Content;

    return content;  
    }  
}
```  
oraz kontroler web api:
```csharp
public class ValuesController : ApiController
{
  public string Get()
  {
      return User.Identity.Name;
  }
}
```
Instalując aplikację na IIS wykonując akcje Index i Go dostaniemy 2 różne wyniki.  
Index zwróci mi aktualnie zalogowanego użytkownika - czyli "Artur", 
a **Go** zwróci użytkownika który jest ustawiony na puli aplikacji.

http://webserver/Home/Go/webserver -> IISPoolUser  
http://webserver/home/index -> Artur

W takim razie jak to zrobić, żeby mimo wszystko http://webserver/Home/Go/webserver zwracał użytkownika Artur?

Po pierwsze do web.config należy dodać wpis:  
```xml
<authentication mode="Windows" />
<identity impersonate="true" />
```

W Active Directory należy ustawić delegację dla użytkownika który jest ustawiony na puli naszej aplikacji.  
Delegację można dodać dopiero wtedy kiedy na naszym użytkowniku jest ustawiony SPN.  
SPN ustawiamy w ten sposób:  
`setspn -s HTTP/nazwa_serwera_www moja_domena\użytkownik_puli`

Aby ustawić delegację w ADUC (Active Directory Users and Computers) wybieramy użytkownika technicznego użytkownik_puli i przechodzimy do zakładki delegacji, która powinna się pojawić po ustawieniu SPN'ów. 
Zaznaczamy "Trust this user for delegation to specified services only" - i dodajemy nową usługę - z znalezionegej maszynę na którym jest zainstalowany nasz IIS i wybieramy usługę HTTP. 

Zapisujemy zmiany i to już koniec.

![]({{ site.baseurl }}/assets/images/delegation.png)

Po tych operacjach http://webserver/Home/Go/webserver i http://webserver/home/index zwracają tego samego użytkownika.

Na 2 hop'y, przekazywanie uprawnień działa bez SPN'a, powyżej 2 "mykiem" jest właśnie SPN żeby zadziałało :smile: