---
layout: post
title: Email Templates w MVC
type: post
categories:
- C#
tags:
- C#
- Email template
permalink: "/email-templates-w-mvc/"
image: 'https://sienicki.dev/assets/tiles/email.png'
category: 'blog' 
introduction: Maile z szablonu w MVC 4
---
Wróciłem ostatnio do zagadnienia wysyłania e-mail z szablonów i bardzo 
nie podobał mi się sposób w jaki to robiłem wcześniej, 
czyli dla każdego template'a nowa metoda if i tak dalej...

W przypadku 2 góra 5 templatów takie rozwiązanie jest bardzo 'lazy' :simple_smile: 
Schody zaczynają się kiedy ich liczba rośnie, a my stwierdzamy, że nie chce nam się :stuck_out_tongue_winking_eye:

W celu ominięcia schodów stworzyłem w projekcie webowych (ASP.NET MVC 4) folder o nazwie EmailTemplates i założyłem, że w tym folderze będą wszystkie HTML templates (np. UserRegisterTemplate.html).

```html
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
       <title></title>
    </head>
    <body>
        Hello
        <b>{UserName}</b>,
        <br/><br/>{ActiveUrl}
    </body>
</html>
```

Stworzyłem w tym folderze również enum'a którego elementami są nazwy templates bez HTML (po co, zaraz się wyjaśni).
```csharp
public enum HtmlTemplate 
{ 
  UserRegisterTemplate 
}
```
Do projektu z modelami dodałem folder Email w którym będziemy trzymać modele 
dla szablonów (być może nazwa nie jest zbyt trafiona, ale zostawiam).

Każdy template z założenia ma jakieś dynamiczne dane. Czy to data, 
czy nazwa użytkownika, nieważne co, zawsze tam coś się trafi :blush:

Założyłem jeszcze, że każda properta/właściwość powinna być typu string, 
dzięki temu będzie łatwiej potem te dynamiczne wartości wstrzyknąć do template'a.

Żebym nie był zależny od konkretnego modelu postanowiłem stworzyć sobie pusty interfejs 
po którym dziedziczą wszystkie modele znajdujące się w folderze email.

```csharp
///<summary> 
/// Wszystkie klasy które dziedziczą z tego interfejsu muszą mieć właściowości które są typu string. 
///</summary> 
public interface IEmail { }
```
oraz model:
```csharp
public class UserRegister : IEmail 
{ 
  public string UserName { get; set; } 
  public string ActiveUrl { get; set; } 
}
```
Z tymi założeniami stworzyłem najważniejszą klasę tej publikacji ConfigureTemplates w folderze EmailTemplates, 
będzie służyła nam ona do tworzenia wiadomości i przekazania dalej do serwisu wysyłającego maile MailMessage i SmtpClient.

```csharp
public class ConfigureTemplates 
{ 
  public static bool SendMail(
                        HtmlTemplate template, 
                        IEmail model, 
                        string recepientEmail, 
                        string subject) 
  { 
      string body = string.Empty; 

      using (StreamReader reader = new StreamReader(HttpContext.Current.Server
                                          .MapPath(string.Format("~/EmailTemplates/{0}.html", 
                                                   template.ToString("g"))))) 
      { 
        body = reader.ReadToEnd(); 
      } 

      foreach (PropertyInfo propertyInfo in model.GetType().GetProperties()) 
      { 
          string oldchar = string.Format("{0}", propertyInfo.Name); 
          string newchar = (string)model
                                    .GetType()
                                    .GetProperty(propertyInfo.Name)
                                    .GetValue(model, null); 
                                    
          body = body.Replace("{" + oldchar + "}", newchar); 
      } 

        using (MailMessage mailMessage = new MailMessage()) 
        { 
          mailMessage.From = new MailAddress(
                            ConfigurationManager.AppSettings["mailLogin"], 
                            ConfigurationManager.AppSettings["SenderName"]); 
          mailMessage.Subject = subject; 
          mailMessage.Body = body; 
          mailMessage.IsBodyHtml = true; 
          mailMessage.To.Add(new MailAddress(recepientEmail)); 
          mailMessage.Priority = MailPriority.Normal; 
          SmtpClient smtp = new SmtpClient(ConfigurationSettings.AppSettings["smtpServer"]) 
                            { 
                                Credentials = new NetworkCredential(
                                                    ConfigurationSettings.AppSettings["mailLogin"], 
                                                    ConfigurationSettings.AppSettings["mailPassword"]) 
                            }; smtp.Send(mailMessage); 
        } 
        
        return true; 
    } 
}
```

Tłumacząc od początku, podaje w parametrach template od którego zależy który plik zostanie wczytany, 
oraz obiekt dziedziczący po interfejsie IEmail, a także email odbiorcy i temat wiadomości.

```csharp
foreach (PropertyInfo propertyInfo in model.GetType().GetProperties())// przechodzi po wszystkich propertach danego obiektu, **(string)
    model
      .GetType()
      .GetProperty(propertyInfo.Name)
      .GetValue(model, null);
      // pobiera wartość tejże property z przeszukiwanego modelu, 
      //a **propertyInfo.Name** dostarcza nam nazwę tej property, szukamy w body string'a który tak się nazywa.
```
Wywołanie metody:

```csharp
ConfigureTemplates.SendMail(HtmlTemplate.UserRegisterTemplate, 
                              new UserRegister 
                                { 
                                  UserName = "Artur", 
                                  ActiveUrl = "blog.rockedge.pl" 
                                }, 
                                "sample@gmail.com", 
                                "Cokolwiek");
```
Oraz dane potrzebne w Web.Config:

```xml
<appSettings>
    <add key="smtpServer" value="smtp.live.com"/>
    <add key="mailLogin" value="ktosiek@live.com"/>
    <add key="mailPassword" value="ktosiekPass"/>
    <add key="SenderName" value="Administrator Ktoś"/>
</appSettings>
```
Tadaa, koniec :simple_smile: 
Na razie jestem zadowolony z tego rozwiązania, zobaczymy co przyjdzie z czasem :simple_smile: