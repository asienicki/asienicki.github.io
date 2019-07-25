---
layout: post
title: Outlook i addin 2/2
type: post
categories:
- C#
tags:
- addin
- exchange
- extended properties
- outlook
permalink: "/outlook-i-addin-2-z-2/"
image: 'https://sienicki.dev/assets/tiles/addin.png'
category: 'blog' 
introduction: jak przekazać dane do exchange’a
---
Zgodnie z zapowiedzią w [pierwszej części](http://sienicki.org/outlook-i-addin-1-z-2/ "Outlook i addin 1/2") dzisiaj pokażę, jak:

1. dodać grupę do istniejącej zakładki
2. podpiąć formularz
3. przekazać istniejący obiekt spotkania do formularza
4. przekazać dane do exchange’a
5. odnaleźć się w dokumentacji

> [Outlook i addin 1/2](http://sienicki.dev/outlook-i-addin-1-z-2/)

**[1]** Aby dodać stworzoną przez nas grupę do istniejącej zakładki wystarczy zrobić tak naprawdę 2 rzeczy:

- zmienić w właściwościach kontrolki RibbonType właściwość z grupy/sekcji/zakładki/? ControlId  -> **ControlIdType**  z Custom na Office
- ustawić OfficeId na istniejące np. **TabAppointment**

Skąd wiedziałem jaką wartość ustawić w OfficeId? Znalazłem w [dokumentacji](http://www.microsoft.com/en-us/download/details.aspx?id=36798). W dokumentacji są pliki xlsx z listami kontrolek i tam w pliku outlookexplorercontrols.xslx ją sobie znalazłem... [wp-svg-icons icon="cool" wrap="i"] Chociaż bardziej cool byłoby gdybym napisał, że zgadłem :tongue:

![OfficeId TabAppointment]({{ site.baseurl }}/assets/images/OfficeIdTabAppointment.png)

Inne przykładowe wartości pola OfficeId:

- None
- TabMail
- TabAttachments
- TabCalendar
- TabContacts
- TabSendReceive
- TabMessage
- TabCalendarTableView
- TabNotes

[Lista kontrolek outlook: outlook explorer controls.xlsx](http://sienicki.dev/assets/xlsx/outlookexplorercontrols.xlsx)

**[2]** Dodawanie własnych formularzy jest proste jak budowa cepa :smile: 
Dodajesz nowy element, wybierasz "Outlook Form Region".

![add new form Addin]({{ site.baseurl }}/assets/images/addnewformAddin.png)

Pojawia się okienko wyboru - wybieramy "Design a new form region".

![add new form Addin]({{ site.baseurl }}/assets/images/addnewformAddin2.png)

Po dodaniu formularza i wrzuceniu na niego testowej "label'ki" uruchamiany aplikację i sprawdzamy efekt. Powinno się pokazać coś takiego:

![OfficeFormRegion]({{ site.baseurl }}/assets/images/OfficeFormRegion.png)

Nasza grupa "Pogodynka" przeniosła się do zakładki spotkanie, a do grupy pokazywanie dodał się formularz.

Przedstawiłem Outlook Form Region tylko dla zajawki [wp-svg-icons icon="smiley" wrap="i"] Teraz go usuwam i dodaje nowy element do projektu z grupy Windows Forms  -> Windows Forms.

W klasie SunTab dodaje metodę ShowForm która ma pokazać świeżo dodaną kontrolkę po kliknięciu przycisku "Sprawdź pogodę".

```csharp
public partial class SunTab 
{ 
  private void BtnCheckWeather(object sender, RibbonControlEventArgs e) 
  { 
    ShowForm(); 
  } 
  
  private HelloForm _form1; 
  
  private void ShowForm() 
  { 
    if (_form1 == null) 
    { 
      _form1 = new HelloForm(); 
    } 
    
    _form1.ShowDialog(); 
  } 
}
```
Zaprojektowałem prosty formularz z kontrolkami numeric i button. 
Do zdarzenia kliknięcia przycisku podpiąłem wyświetlanie kolejnego okna.

```csharp
private void ButtonPrintTemperature(object sender, EventArgs e) 
{ 
  MessageBox.Show($"Dzień dobry! Dziś na dworzu {ageNumeric.Value} stopni :)"); 
}
```

Odpalamy aplikację i rzeczywiście, mamy w naszej grupie przycisk, 
który po kliknięciu reaguje stworzeniem nowego okna, 
w którym możemy podać temperaturę, a następnie ją wyświetlić :tongue:

![kontrolkaWindowsFormsOutlook]({{ site.baseurl }}/assets/images/kontrolkaWindowsFormsOutlook.png)

**[3]** Ok, mamy już kilka ciekawych rzeczy związanych głównie z GUI, ale jak przekazać dane? 
Oczywiście możemy dodać nowy formularz, który będzie dodawał coś do tytułu/treści wiadomości, ale to nie jest fajne.

Fajne byłoby dodanie do wiadomości, naszego własnego atrybutu, do którego moglibyśmy wrzucić co tylko nam się podoba.

Spotkanie w Outlooku to nic innego jak wiadomość email, do której można dodać swoje "extended properties".
Wspomniane "extended properties" obsługiwane są jedynie przez Exchange'a.

Pierwszym krokiem jest uczynienie publicznym i przekazanie do naszego formularza referencji do obiektu spotkania.
W głównej klasie AddIn'a dodajemy inspektora, który przy starcie ustawi nam obiekt spotkania.

```csharp
using Outlook = Microsoft.Office.Interop.Outlook; 

public partial class ThisAddIn 
{ 
    private Outlook.Inspectors _inspectors; 
    public Outlook.AppointmentItem AppointmentItem; 

    private void ThisAddIn_Startup(object sender, System.EventArgs e) 
    { 
        _inspectors = Application.Inspectors;
        _inspectors.NewInspector += Inspectors_NewInspector;
    } 

    private void Inspectors_NewInspector(Outlook.Inspector inspector) 
    { 
        var appointmentItem = inspector.CurrentItem as Outlook.AppointmentItem; 
        if (appointmentItem != null) 
        { 
          AppointmentItem = appointmentItem; 
        } 
    }
```
W zakładce SunTab przechodzimy do metody pokazującej formularz i przekazujemy spotkanie np. przez konstruktor.

```csharp
private void ShowForm() 
{ 
  if (_form1 == null) 
  { 
    _form1 = new HelloForm(Globals.ThisAddIn.AppointmentItem); 
  } 
  
  _form1.ShowDialog(); 
}
```

Po czym dodajemy do niego parametr:

```csharp
private readonly AppointmentItem _appointmentItem; 

public HelloForm(AppointmentItem appointmentItem) 
{ 
  _appointmentItem = appointmentItem; InitializeComponent(); 
}
```
**[4]**  Teraz wystarczy tylko do obiektu spotkania dodać coś od siebie. Jak to zrobić? 
Również całkiem prosto - pod warunkiem, że się wie, albo ma się dobry przykład :cool:

Do formularza dodajemy metodę inicjowania właściwości i modyfikujemy metodę wyświetlającą wiek. 
Teraz będzie również przekazywała tajną informację :wink:

```csharp
private void ButtonPrintTemperature(object sender, EventArgs e) 
{ 
  MessageBox.Show($"Dzień dobry! Dziś na dworzu {ageNumeric.Value} stopni :)"); 
  var properties = _appointmentItem.UserProperties; 
  var secretData = new SecretData 
  { 
    Age = ageNumeric.Value 
  }; 
  
  try 
  { 
    InitProperty(properties, secretData); 
  } catch (UnauthorizedAccessException) 
  { 
    InitProperty(properties, secretData); 
  } 
} 
  
private static void InitProperty(UserProperties properties, SecretData secretData) 
{ 
    var property = properties.Add("secretData", OlUserPropertyType.olText); 
    property.Value = JsonConvert.SerializeObject(secretData); 
}
```

Nie pytajcie dlaczego zrobiłem dwa razy InitProperty w try catch... to exchange :worried: 
Z jakiegoś magicznego powodu pojedyńcza inicjalizacja rzuca wyjątek - _rozwiązanie_ jest banalne... try catch i po sprawie... trudno.

W każdym razie, teraz po kliknięciu na przycisk, powinna się dodać do wiadomości wysyłanej do Exchange'a, informacja o wieku. 
Jeżeli macie Exchange'a, to można to w łatwy sposób sprawdzić, a jeżeli nie... to cóż :tongue:

**[5]** Poruszanie się po dokumentacji jest dosyć ciekawe i niestety nie ma na to złotego środka. 
Jedyny tip od mnie to ten zawarty w punkcie pierwszym. 
Jeżeli szukasz - dajmy na to... nazwy zakładki zgadnij ją sobie w excelu :grinning:

> **Have Fun! **

[Projekt w całości dostępny na github](http://github.com/asienicki/HelloAddin) :smile: