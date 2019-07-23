---
layout: post
title: XSS injection a MVC
type: post
categories:
- Bez kategorii
permalink: "/xss-injection-a-mvc/"
image: 'https://sienicki.dev/assets/tiles/service.jpg.jpg'
category: 'blog' 
introduction: XSS MVC
---
Domyślnie samo ASP.NET MVC zabezpiecza nas przed XSS injection, ale możemy to zabezpieczenie mniej lub bardziej świadomie wyłączyć i umożliwić wykradnięcie zmiennej sesyjnej czy też danych użytkowników przez dodanie atrybutu ValidateInput(false).

Ostatnio spotkałem się z przypadkiem gdzie ValidateInput(false) został użyty w celu zezwolenia przechowywania treści HTML'a w polu na formularzu. Spoko. 

Z tym, że walidacji na javascript też przy okazji się pozbyliśmy.

Szybkie lekarstwo? dodaj atrybut AllowHtml do właściwości która może zawierać HTML'a.