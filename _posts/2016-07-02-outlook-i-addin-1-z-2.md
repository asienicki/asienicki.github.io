---
layout: post
title: Outlook i addin 1/2
type: post
status: publish
categories:
- C#
tags:
- addin
- outlook
- ribbon
permalink: "/outlook-i-addin-1-z-2/"
image: 'https://sienicki.dev/assets/tiles/addin.png'
category: 'blog' 
introduction: Udało mi się napisać pierwszy w życiu addin do outllooka. 
---
Udało mi się napisać pierwszy w życiu addin do outllooka. 
Nie wyobrażam sobie żeby nie uczcić takiej okazji postem ozdobionym tutorialem ([projekt na GITHUB](http://github.com/asienicki/HelloAddin)).

Zaczyna się dosyć prosto. Wybieramy nowy projekt w Visual Studio z menu Office/SharePoint -> Office Add-ins -> Outlook 2013 and 2016 VSTO Add-in: ![nowy project add-in]({{ site.baseurl }}/assets/images/2016-07-01.png)

Po stworzeniu projektu widzimy klasę ThisAddIn z tak naprawdę 1 metodą. Nic specjalnego. 
Shutdown'a mamy nie używać [wp-svg-icons icon="smiley" wrap="i"]

```csharp
public partial class ThisAddIn 
{ 
  private void ThisAddIn_Startup(object sender, System.EventArgs e) 
  { 
  }
```

Sprawdźmy teraz co możemy do naszego pluginu dodać przez stworzenie nowego elementu.

![addin items]({{ site.baseurl }}/assets/images/2016-07-01-2.png)

Jak widać na powyższym zrzucie możemy dodać Region albo Ribbon.

Ribbon jest to nowa zakładka/wstążka która pojawi się po uruchomieniu plugginu. 
Możemy wybrać dwa rodzaje Ribbona xml i designer. Zaczynamy zabawę z Addinem - więc designer.

Dodaje element Ribbon (visual designer) który nazywamy SunTab :sun_with_face:

![ribbon visual designer]({{ site.baseurl }}/assets/images/2016-07-01-4.png)

i moim oczom ukazuje się taki piękny widok:

![zakładka designer SunTab]({{ site.baseurl }}/assets/images/SunTab.png)

To co możemy zrobić to ustawić właściwości samej zakładki - klikamy w obszarze "najwyższego poziomu" na kontrolkę OfficeRibon i  przechodzimy do properties przez skrót F4 albo right click :tongue:

To co jest najważniejsze w tej chwili dla nas to właściwość RibbonType.

![ribbon type]({{ site.baseurl }}/assets/images/ribbon-type.png)

Możemy w niej zdecydować gdzie nasza zakładka ma się pojawiać. 
Każdy tutorial jest z mailami, to ja dla odmiany pokażę jak to zrobić z spotkaniami.

Wybieramy **Microsoft.Outlook.Appointment** oraz **Microsoft.Outlook.MeetingRequest.Send**.

OfficeRibon posiada kolekcję zakładek. Każdej zakładce możemy ustawić nazwę, etykietę i controlId.

O ile etykieta i nazwa dla funkcjonowania addina jest nieistotna to controlId już jest. 
Zależnie od tego jak ustawimy ControlIdType Office/Custom to mamy możliwość kontrolowania 
czy dodajemy zawartość naszej zakładki do już istniejącej, czy tworzymy zupełnie nową zakładkę. 
Na razie spróbujmy stworzyć własną zakładkę.

![RibbonTab Custom]({{ site.baseurl }}/assets/images/RibbonTab-Custom.png)

Przejdźmy do samej zawartości naszej zakładki. 
Można do niej dodawać grupy, a do każdej grupy kontrolki. 
Na przykład takie:

![ribbon group controls]({{ site.baseurl }}/assets/images/ribbon-group-controls.png)

Tam gdzie na zakładce jest napis group1 przeciągam kontrolkę button i ustawiam jej tytuł i chcę obrazek.

Żeby mieć obrazek należy go dodać do projektu z właściwością build action 
ustawioną na none i przeciągnąć go do zasobów aplikacji.

![sun resource]({{ site.baseurl }}/assets/images/sun-resource.png)

Po modyfikacjach panelu grupy u mnie w designerze wygląda to teraz tak:

![sunTab 1 version]({{ site.baseurl }}/assets/images/sunTab-1-version.png)

Zrobiłem dwuklik na przycisku i automatycznie stworzył się event jak na WinFormsowo podobny twór przystało. 
Do treści zdarzenia dodaje prosty alert.

```csharp
private void button1_Click(object sender, RibbonControlEventArgs e) 
{ 
    MessageBox.Show("Jutro znowu gorąco! Kup klimę!"); 
}
```

Zasłużyliśmy, żeby zobaczyć efekt naszej pracy. 
Rozpocznijcie debugowanie. 
Outlook uruchomi się automatycznie. 
Wybierzcie kalendarz, nowe spotkanie i zakładkę "Pogoda".

Teraz kliknijcie przycisk "Sprawdź pogodę"- jeśli wyświetla się alert - jest wszystko dobrze, 
a jeśli nie to zepsuliście - wróćcie na początek :tongue:

![test addin]({{ site.baseurl }}/assets/images/test-addin.png)

Wszystko pięknie, ale jak...

1. dodać grupę do istniejącej zakładki?
2. podpiąć formularz?
3. przekazać dane z formularza do istniejącego obiektu spotkania?
4. przekazać dane do exchange'a?
5. odnaleźć się w dokumentacji?

Na powyższe pytania znajdziecie odpowiedzi w kolejnym moim wpisie dotyczącym addinów. Mam nadzieję, że się podobało i nie było zbyt szczegółowo [wp-svg-icons icon="smiley" wrap="i"]

Testowy projekt jest do pobrania na [githubie: http://github.com/asienicki/HelloAddin](http://github.com/asienicki/HelloAddin)

http://sienicki.dev/outlook-i-addin-2-z-2/