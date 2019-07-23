---
layout: post
title: Łańcuch odpowiedzialności
type: post
categories:
- C#
- Design Patterns
tags:
- C#
- chain of responsibility
- łańcuch odpowiedzialności
permalink: "/lancuch-odpowiedzialnosci/"
image: 'https://sienicki.dev/assets/tiles/strategia.jpg'
category: 'blog' 
introduction: Nullowy operator koalescencyjny
---
 **_Książkowa definicja_:** _łańcuch odpowiedzialności (chain of responsibility) - służy do stworzenia łańcucha obiektów, które przetwarzają żądanie. Przetwarzanie żądania odbywa się w każdym obiekcie należącym do łańcucha odpowiedzialności. Obiekt obsługuje żądanie albo wysyła je dalej._

**_Kiedy:_**

- piszemy program który musi wykonać pewne zadania sekwencyjnie, jedno za drugim
- zachodzi potrzeba zamienienia kolejności wykonywania zadań lub tymczasowego pominięcia wykonywania niektórych z nich

Oczywiście można zrobić wszystko w 1 metodzie, albo w 1 klasie z użyciem kilku metod, ale najbardziej przejrzystym sposobem według mnie jest przypisanie zadania do klasy, dzięki temu stosujemy się do 1 zasady z SOLID - S - pojedynczej odpowiedzialności.

Poniżej przygotowałem wstępną implementację wzorca 'łańcuch odpowiedzialności' całkowicie po polsku tylko dla przykładu.

**1 krok**, stworzenie odpowiednich interfejsów i klasy stanu.

```csharp
public interface IZadanie
{
    void Wykonaj(Stan stan);
}

public interface IFabrykaZadan  
{  
    ZadanieKompozytowe ZadanieKompozytowe(params IZadanie[] zadanie);

    PierwszeZadanie PierwszeZadanie();

    DrugieZadanie DrugieZadanie();  
}

public class Stan  
{  
    public Type Typ { get; set; }  
}
```

Żadnego "rocket since" - nasze zadanie ma coś wykonywać na podstawie stanu. 
FabrykaZadan ma nam dostarczać gotowe zadania, co by nie musieć samemu sobie tworzyć instancji obiektów.
Stan to prosty kontener na dane dodajemy tam coś co jest obecne w każdym zadaniu. Można np. dodać **żądanie**, flagę isError, listę komunikatów, czas wykonania, długość wykonania (per zadanie) i etc.

Implementacje dla naszych interfejsów wykonałem tak jak poniżej.

```csharp
public class FabrykaZadan : IFabrykaZadan 
{ 
  public ZadanieKompozytowe ZadanieKompozytowe(params IZadanie[] zadania) 
  { 
    return new ZadanieKompozytowe(zadania); 
  } 
  public PierwszeZadanie PierwszeZadanie() 
  { 
    return new PierwszeZadanie(); 
  } 
  
  public DrugieZadanie DrugieZadanie() 
  { 
    return new DrugieZadanie(); 
  } 
} 

public class PierwszeZadanie : IZadanie 
{ 
  public void Wykonaj(Stan stan) 
  { 
    stan.Typ = typeof(PierwszeZadanie);
     //coś robię 
     Console.WriteLine("1"); 
  } 
} 

public class DrugieZadanie : IZadanie 
{ 
  public void Wykonaj(Stan stan) 
  { 
    stan.Typ = typeof(DrugieZadanie); 
    //coś robię 
    Console.WriteLine("2"); 
  }
}
```

Na 1 rzut oka **strasznie/obrzydliwie/okropnie** wygląda fabryka zadań, jak na to patrzę to dostaje gęsiej skórki :wink: ale zignorujcie to na chwilę - moja fabryka ma 1 cel  -> dostarczyć instancję obiektu - i to właśnie robi. 
Mam na to lepsze rozwiązanie, znajdziesz je w _ciekawostka_.

Dodałem również nowe zadania. 
Pierwsze i drugie są zadaniami które robią "to coś" co do nich należy. :simple_smile:

Natomiast ciekawszym zadaniem jest zadanie kompozytowe które umożliwia zbudowanie drzewka zadań. Jego implementacja jest prosta. Dostaje w parametrze listę zadań które muszę po kolei wykonać.

```csharp
public class ZadanieKompozytowe : IZadanie 
{ 
  private readonly IZadanie[] _zadania; 
  
  public ZadanieKompozytowe(IZadanie[] zadania) 
  { 
    _zadania = zadania; 
  } 
  
  public void Wykonaj(Stan stan) 
  { 
    if (stan == null) 
    { 
      stan = new Stan(); 
    } 
    
    foreach (var zadanie in _zadania) 
    { 
      zadanie.Wykonaj(stan); 
      } 
    } 
}
```
Pozostaje jedynie przetestować działanie naszego łańcucha.

```csharp
class Program 
{ 
  static void Main(string[] args) 
  { 
      IFabrykaZadan fabryka = new FabrykaZadan(); 
      
      var zadania = fabryka.ZadanieKompozytowe(
                              fabryka.PierwszeZadanie(), 
                              fabryka.DrugieZadanie(), 
                              fabryka.ZadanieKompozytowe(
                                fabryka.ZadanieKompozytowe(
                                fabryka.DrugieZadanie()), 
                              fabryka.PierwszeZadanie())); 
            
      zadania.Wykonaj(new Stan()); Console.ReadKey(); 
  } 
}
```
Stworzyłem fabrykę zadań z której pobieram zadanie kompozytowe i dalej wykorzystuje fabrykę do sterowania kolejnością wykonywania zadań. 
Takie zaprojektowanie aplikacji zapewnia dużą przejrzystość kodu i o ile nie będziemy wykorzystywać stanu jako obiektu do przekazywania danych między zadaniami kolejność wykonywania zadań może być dowolna.

Efektem uruchomienia programu będzie oczywiście:
- 1
- 2
- 2
- 1

_ **Przykład:** _ Idealnym przykładem z życia do zastosowania wzorca łańcucha odpowiedzialności jest parsowanie stron HTML z jakimiś losowymi danymi. Proces parsowania można podzielić na istotne etapy _(w przypadku parsowania oczywiście odpada nam losowa kolejność wykonywania zadań, ale dajmy na to, że wykonywaliśmy krok konwertowania danych z base64, a na parsowanej stronie dane przestały być zamieniane do base'a, jedyne co musimy zrobić to za komentować (być może zaraz się znowu zmieni - warto dodać też komentarz dlaczego) wykorzystanie  zadania odczytującego base'a)_.

**_Ciekawostka:_** Jedna rzecz na koniec odnośnie mojej fabryki zadań. Każdy chyba stwierdził, że jest ona _"strasznie nie fajna"_. W komercyjnych projektach zamiast tego używam rozszerzenia Ninject.Factory które pozwala mi zarejestrować sam interfejs IFabrykaZadan jako fabrykę. Po wstrzyknięciu przez kontener IFabrykaZadan mamy obiekt który zwraca nam w momencie wywołania metody instancję obiektu dla zadania. Rozwiązanie to pozwala nam na pozbycie się klasy FabrykaZadan która w sumie nic nie robi, a trzeba ją utrzymywać.