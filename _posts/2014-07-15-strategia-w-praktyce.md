---
layout: post
title: Strategia w praktyce
type: post
categories:
- Design Patterns
tags:
- design patterns
- strategia
- strategy pattern
- wzorzec projektowy
permalink: "/strategia-w-praktyce/"
image: 'https://sienicki.dev/assets/tiles/strategia.jpg'
category: 'blog' 
introduction: Przykład z użyciem strategii
---
Jakiś czas temu oczywiście w celach rozrywkowych potrzebowałem prostego narzędzia, a niczego sensownego nie mogłem znaleźć w nugecie, 
do zmiany wartości storage entity pattern na computed dla wszystkich właściwości które nazywają się CreateTS lub UpdateTS.

Nic prostszego. Niestety nie mam pierwszej wersji aplikacji, ani kolejnych (nie czułem potrzeby 'wersjonować' sobie tego), ale mam wersję finalną którą się chętnie podzielę.

Stworzyłem aplikację konsolową która jako parametry wejściowe przyjmuje argumenty:

- arg0 - rodzaj programu (w tej chwili jest tylko 1 dostępne polecenie - computed, dzięki temu możemy w łatwy sposób dodawać kolejne polecenia do naszego programu)
- arg1 - przechowuje ścieżkę do naszego pliku edmx
- arg2,..,argN - lista parametrów które zostają, w przypadku polecenia computed są to właściwości datetime które chcemy zmienić

Aplikacja nie zwraca żadnych danych, jedyne co robi to edytuje plik edmx.

Kiedy wiemy co do nas przychodzi możemy zacząć działać :simple_smile:

Pierwszą rzeczą którą należy zrobić jest znalezienie klasy wykonawczej, odpowiedniej dla polecenia. 
Normalnie przy tłumaczeniu strategy pattern w tym miejscu pojawiłyby się instrukcje warunkowe if, wiele, żeby pokazać jakie to złe podejście, więc.. wyobraźcie sobie dużo if'ów, a ja wam pokażę "ExecutorResolver".

```csharp
public class ExecutorResolver 
{ 
  public IExecutor Resolve(string programKind, string pathEdmx) 
  {     
      IExecutor executor; 
      
      switch (programKind) 
      { 
          case "computed": 
            executor = new DateTimeComputedExecutor(); 
            break; 
          default: 
            executor = new DefaultExcutor(); 
            break; 
      } 

      executor.Path = pathEdmx; return executor; 
  } 
}
```

Tak, tak, magia, zamiast if'ów mamy switcha. 
Dodatkowo inicjuje w wykonawcy kimkolwiek on by nie był właściwość Path (można tu, można w main, preferuje jednakże w środku).

Stworzyłem dwóch wykonawców z czego interesuje nas: DateTimeComputed.

Obaj wykonawcy dziedziczą po interfejsie IExecutor, to przez niego wykonawcy są zmuszeni do zaimplementowania metody Execute i właściwość Path.

```csharp
public interface IExecutor 
{ 
    string Path { get; set; } 

    void Execute(); 

    void Execute(string parameter); 

    void Execute(IList<string> parameters);
}
```

I nasz wykonawca:

```csharp
public class DateTimeComputedExecutor : IExecutor 
{ 
  public string Path { get; set; } 
  
  public void Execute() { } 
  
  public void Execute(string parameter) { } 
  
  public void Execute(IList<string> dateTimeProperties) 
  { 
    var streamReader = File.OpenText(Path); 
    var contents = streamReader.ReadToEnd(); 
    streamReader.Close(); 
    StreamWriter streamWriter = File.CreateText(Path); 
    var newFile = new StringBuilder(); 
    var strings = contents.Split(new[] { "\r\n" }, StringSplitOptions.None); 
    
      foreach (var line in strings) 
      { 
            var newLine = line; 
            
            foreach (var property in dateTimeProperties) 
            {
              var stringToReplace = string.Format("<Property Name=\"{0}\" Type=\"datetime2\">", property); 
              var stringToReplace2 = string.Format("<Property Type=\"DateTime\" Name=\"{0}\" Precision=\"7\" />", property); 
              
              if (line.Contains(stringToReplace)) 
              { 
                newLine = line.Replace(stringToReplace, 
                string.Format( "<Property Name=\"{0}\" Type=\"datetime2\" StoreGeneratedPattern=\"Computed\" />", property)); 
              } 
              if (line.Contains(stringToReplace2)) 
              { 
                newLine = line.Replace(stringToReplace2, 
                string
                .Format( "<Property Type=\"DateTime\" Name=\"{0}\" Precision=\"7\" annotation:StoreGeneratedPattern=\"Computed\" />", property));
              } 
          } 
          newFile.Append(newLine); 
          streamWriter.WriteLine(newLine); 
      }     
    
      streamWriter.Close(); 
  } 
}
```
Wykonawca już robi co do niego należy, czyli zastępuje odpowiednie linie w xml edmx'a.
I na koniec metoda main:

```csharp
class Program 
{ 
  static void Main(string[] args) 
  { 
      if (args.Length <= 2) 
      return; 
      
      var excutorResolver = new ExecutorResolver(); 
      
      var programKind = args[0]; 
      var edmxPath = args[1]; 
      var arguments = args.Skip(2).ToList(); 
      
      IExecutor executor = excutorResolver.Resolve(programKind, edmxPath); 
      executor.Execute(arguments); 
  } 
}
```
Podsumowując, zyskujemy przejrzystość, prostotę przy dodaniu kolejnej strategii i możliwość łatwego przetestowania każdej strategii z osobna.