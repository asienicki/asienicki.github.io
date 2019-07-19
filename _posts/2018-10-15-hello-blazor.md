---
layout: post
title: Hello Blazor
type: post
categories:
- Blazor
tags:
- Blazor
- WebAssembly
permalink: "/hello-blazor/"  
image: 'https://asienicki.github.io/assets/img/morpheus_xdzgg1.jpg'
category: 'blog' 
introduction: W ostatnich dniach na "ołpen spejsach" i "salonach" tematem numer 1 jest WebAssembly.
---

W ostatnich dniach na "ołpen spejsach" i "salonach internetowych" tematem numer 1 jest WebAssembly.

Zaciekawiony, postanowiłem przyjrzeć się tematowi bliżej.

Nowy twór MSu - [Blazor](http://blazor.net/) jest implementacją [WebAssembly](http://webassembly.org/) w .NET.

Jak to [wygląda w praktyce](http://blazor.net/docs/get-started.html)?

Po pierwsze musimy mieć zainstalowany [.NET Core 2.1](http://www.microsoft.com/net/download) (\>=2.1.402). Przyda się też rozszerzenie do naszego Visual Studio:&nbsp;&nbsp;[ASP.NET Core Blazor Language Services.](http://marketplace.visualstudio.com/items?itemName=aspnet.blazor)

Tworzymy nowy projekt typu " **ASP.NET Core Web Application**" i wybieramy szablon "Blazor".

F5 i **hello world** zrobił się sam.

![hello World Blazor]({{ site.baseurl }}/assets/images/helloWorldBlazor.png)

Serwer zwrócił 13 plików z roszerzeniem dll.
Pierwszą rzeczą jaką zapragnąłem zrobić było pobranie pliku "_BlazorHelloWorld.WebApp.dll_" i otworzeniu go/jej w dotpeek :)

Porównałem zawartość dllki z tym co znajduje się w projekcie i mamy odwzorowanie 1 do 1. 
Każdy widok (to dalej jest widok?;)) - każdy plik o rozszerzeniu cshtml odpowiada klasie.

![]({{ site.baseurl }}/assets/images/helloWorldBlazorDllVsCode.png)

Na pierwszy ogień dekompiluje klasę ViewImport. Rezultat niżej.

```csharp
using BlazorHelloWorld.WebApp.Shared; 
using Microsoft.AspNetCore.Blazor.Components; 
using Microsoft.AspNetCore.Blazor.Layouts; 
using Microsoft.AspNetCore.Blazor.RenderTree; 

namespace BlazorHelloWorld.WebApp.Pages 
{
    [Layout(typeof(MainLayout))] 
    public class ViewImports : BlazorComponent 
    { 
        protected override void BuildRenderTree(RenderTreeBuilder builder) 
        { 
          base.BuildRenderTree(builder); 
        } 
    } 
}
```

Zaciekawił mnie tylko atrybut Layout. Cieszy mnie podejście komponentowe, a tak poza tym zgodnie z przewidywaniami ta klasa poza podtrzymaniem życia BuildRenderTree nic nie robi.

Layout:

```csharp
// Decompiled with JetBrains decompiler 
// Type: BlazorHelloWorld.WebApp.Shared.MainLayout 
// Assembly: BlazorHelloWorld.WebApp, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null 
// MVID: 5CE28297-B925-4548-A5E1-DAFA2DB6D448 

using Microsoft.AspNetCore.Blazor.Layouts; 
using Microsoft.AspNetCore.Blazor.RenderTree; 

namespace BlazorHelloWorld.WebApp.Shared 
{ 
  public class MainLayout : BlazorLayoutComponent 
  { 
      protected override void BuildRenderTree(RenderTreeBuilder builder) 
      { 
          base.BuildRenderTree(builder); 

          builder.OpenElement(0, "div"); 
          builder.AddAttribute(1, "class", "sidebar"); 
          builder.AddContent(2, "\n "); 
          builder.OpenComponent<NavMenu>(3); 
          builder.CloseComponent(); 
          builder.AddContent(4, "\n"); 
          builder.CloseElement(); 
          builder.AddContent(5, "\n\n"); 
          builder.OpenElement(6, "div"); 
          builder.AddAttribute(7, "class", "main"); 
          builder.AddContent(8, "\n "); 
          builder.AddMarkupContent(9, "\<div class=\"top-row px-4\"\>\n \<a href=\"http://blazor.net\" target=\"\_blank\" class=\"ml-md-auto\"\>About\</a\>\n \</div\>\n\n "); 
          builder.OpenElement(10, "div"); 
          builder.AddAttribute(11, "class", "content px-4"); 
          builder.AddContent(12, "\n "); 
          builder.AddContent(13, this.Body); 
          builder.AddContent(14, "\n "); 
          builder.CloseElement(); 
          builder.AddContent(15, "\n"); 
          builder.CloseElement();
      } 
  } 
}
```

W szablonie dzieje się już jakaś akcja :smile:
Widzimy, że Layout dziedziczy po innej klasie niż ViewImports.

W BuildRenderTree najpierw wykonuje bazową metodę budowania drzewa, 
a następnie drzewo jest rozbudowywane o elementy html'a.

Na uwagę zasługuje:

```csharp
builder.OpenComponent<NavMenu>(3);
```

W końcu to coś innego niż tworzenie elementów html i dodawanie do nich atrybutów.

NavMenu:

```csharp
public class NavMenu : BlazorComponent 
{ 
  private bool collapseNavMenu = true; 
  
  protected override void BuildRenderTree(RenderTreeBuilder builder) 
  { 
        base.BuildRenderTree(builder);
        builder.OpenElement(0, "div"); 
        builder.AddAttribute(1, "class", "top-row pl-4 navbar navbar-dark"); 
        builder.AddContent(2, "\n "); 
        builder.AddMarkupContent(3, "\<a class=\"navbar-brand\" href=\"\"\>BlazorHelloWorld.WebApp\</a\>\n "); 
        builder.OpenElement(4, "button"); 
        builder.AddAttribute(5, "class", "navbar-toggler"); 
        builder.AddAttribute(6, "onclick", BindMethods.GetEventHandlerValue<UIMouseEventArgs>(new Action(this.ToggleNavMenu)));
        builder.AddMarkupContent(7, "\n \<span class=\"navbar-toggler-icon\"\>\</span\>\n "); 
        builder.CloseElement(); 
        builder.AddContent(8, "\n"); 
        builder.CloseElement(); 
        builder.AddContent(9, "\n\n"); 
        builder.OpenElement(10, "div"); 
        builder.AddAttribute(11, "class", this.collapseNavMenu ? "collapse" : (string) null); 

        builder.AddAttribute(12, "onclick", 
        BindMethods.GetEventHandlerValue<UIMouseEventArgs>(new Action(this.ToggleNavMenu))); 

        builder.AddContent(13, "\n "); 
        builder.OpenElement(14, "ul"); 
        builder.AddAttribute(15, "class", "nav flex-column"); 
        builder.AddContent(16, "\n "); 
        builder.OpenElement(17, "li"); 
        builder.AddAttribute(18, "class", "nav-item px-3"); 
        builder.AddContent(19, "\n "); 
        builder.OpenComponent<NavLink>(20); 
        builder.AddAttribute(21, "class", "nav-link"); 
        builder.AddAttribute(22, "href", ""); 
        builder.AddAttribute(23, "Match", (object) RuntimeHelpers.TypeCheck\<NavLinkMatch\>(NavLinkMatch.Prefix));
        builder.AddAttribute(24, "ChildContent", (MulticastDelegate) (builder2 => 
                  builder2.AddMarkupContent(25, "\n \<span class=\"oi oi-home\" aria-hidden=\"true\"\>\</span\> Home\n "))); 
        builder.CloseComponent(); 
        builder.AddContent(26, "\n "); 
        builder.CloseElement(); 
        builder.AddContent(27, "\n "); 
        builder.OpenElement(28, "li"); 
        builder.AddAttribute(29, "class", "nav-item px-3"); 
        builder.AddContent(30, "\n "); 
        builder.OpenComponent<NavLink>(31); 
        builder.AddAttribute(32, "class", "nav-link"); 
        builder.AddAttribute(33, "href", "counter"); 
        builder.AddAttribute(34, "ChildContent", (MulticastDelegate) (builder2 => 
                  builder2.AddMarkupContent(35, "\n \<span class=\"oi oi-plus\" aria-hidden=\"true\"\>\</span\> Counter\n "))); 
        builder.CloseComponent(); 
        builder.AddContent(36, "\n "); 
        builder.CloseElement(); 
        builder.AddContent(37, "\n "); 
        builder.OpenElement(38, "li"); 
        builder.AddAttribute(39, "class", "nav-item px-3"); 
        builder.AddContent(40, "\n "); 
        builder.OpenComponent<NavLink>(41); 
        builder.AddAttribute(42, "class", "nav-link"); 
        builder.AddAttribute(43, "href", "fetchdata"); 
        builder.AddAttribute(44, "ChildContent", (MulticastDelegate) (builder2 => builder2.AddMarkupContent(45, "\n \<span class=\"oi oi-list-rich\" aria-hidden=\"true\"\>\</span\> Fetch data\n "))); 
        builder.CloseComponent(); 
        builder.AddContent(46, "\n "); 
        builder.CloseElement(); 
        builder.AddContent(47, "\n "); 
        builder.CloseElement(); 
        builder.AddContent(48, "\n"); 
        builder.CloseElement(); 
  }

  private void ToggleNavMenu() 
  { 
        this.collapseNavMenu = !this.collapseNavMenu; 
  } 
}
```

Sporo kodu w zdecydowanej większość proste budowanie elementów, ale przyjrzyjmy się bliżej wybranym fragmentom.

```csharp
private bool collapseNavMenu = true; 

builder.AddAttribute(11, "class", this.collapseNavMenu ? "collapse" : (string) null); 
builder.AddAttribute(6, "onclick", BindMethods.GetEventHandlerValue<UIMouseEventArgs>(new Action(this.ToggleNavMenu))); 

private void ToggleNavMenu() 
{ 
  this.collapseNavMenu = !this.collapseNavMenu; 
}
```

Mamy tutaj użytą metodę onclick podpiętą do zdarzenia&nbsp;ToggleNavMenu. Nice... nazwa "[UIMouseEventArgs](http://blazor.net/api/Microsoft.AspNetCore.Blazor.UIMouseEventArgs.html)" pachnie trochę wpf'em.

Zobaczmy jak to zrobić od jedynej słusznej strony. NavMenu.cshtml:

```html
<button class="navbar-toggler" onclick=@ToggleNavMenu>
  <span class="navbar-toggler-icon"></span> 
</button>

<div class=@(collapseNavMenu ? "collapse" : null) onclick=@ToggleNavMenu>

<ul class="nav flex-column">
  <li class="nav-item px-3">
    <NavLink class="nav-link" href="" Match=NavLinkMatch.Prefix>
      <span class="oi oi-home" aria-hidden="true"></span>
      Home
    </NavLink>
  </li> 
  <li class="nav-item px-3">
    <NavLink class="nav-link" href="counter">
      <span class="oi oi-plus" aria-hidden="true"></span> 
      Counter
    </NavLink>
  </li>
</ul>
</div> 

```
```csharp
@functions 
{ 
  bool collapseNavMenu = true; 
  
  void ToggleNavMenu() { 
    collapseNavMenu = !collapseNavMenu; 
  } 
}
```

Wygląda prosto. Mamy sekcję _functions_, która pozwala nam na stworzenie metody i zmiennych. 
Z poziomu razor'a podpinamy do metody onclick zdefiniowaną niżej metodę.

A jak to wygląda w html'u?

```html
<button class="navbar-toggler">
  <span class="navbar-toggler-icon"></span> 
  </button>
  
  <ul class="nav flex-column">
    <li class="nav-item px-3">
      <a class="nav-link active" href="" match="Prefix">
        <span class="oi oi-home" aria-hidden="true"></span>Home</a>
    </li>
    <li class="nav-item px-3"> 
          <a class="nav-link" href="counter">
            <span class="oi oi-plus" aria-hidden="true"></span> Counter</a>
    </li>
  </ul>
```

Żadnych onclick'ów bezpośrednio w javascript. 
Zdarzenie zarejestrowane jest w webAssembly. 
Rąbka tajemnicy uchyliłoby przeanalizowanie plików wasm, ale są to po prostu binarki.

Kolejny fragment wart uwagi to komponent NavLink.

```csharp
builder.OpenComponent<NavLink>(41); 
builder.AddAttribute(42, "class", "nav-link"); 
builder.AddAttribute(43, "href", "fetchdata"); 
builder.AddAttribute(44, "ChildContent", (MulticastDelegate) (builder2 => 
              builder2.AddMarkupContent(45, "\n\<span class=\"oi oi-list-rich\" aria-hidden=\"true\"\>\</span\> Fetch data\n"))); 
builder.CloseComponent();
```

NavLink jest to wbudowany w blazora komponent do tworzenia linków.

W pliku NavMenu.cshtml wygląda to tak:

```html
<NavLink class="nav-link" href="" Match=NavLinkMatch.Prefix>
  <span class="oi oi-home" aria-hidden="true"></span>Home
</NavLink>
```

Można mu ustawić atrybut Match (niestety intellisense nie podpowiedziało mi wszystkich wartości (na dziś są to All i Prefix)). 
Kiedy url pasuje do aktualnego adresu do linku jest dodawana klasa "active".

```html
<ul class="nav flex-column">
  <li class="nav-item px-3"> 
    <a class="nav-link active" href="counter">
      <span class="oi oi-plus" aria-hidden="true"></span>Counter
    </a>
  </li>
</ul>
```

Więcej o navlink:
1. [dokumentacja](http://blazor.net/api/Microsoft.AspNetCore.Blazor.Routing.NavLink.html)
2. [kurs](http://learn-blazor.com/pages/router/#links)

Koncepcja jest bardzo ciekawa i nie ukrywam, że mi się podoba.
W końcu po dłuższej przerwie zdecydowałem się napisać artykuł akurat o Blazor'ze ;)