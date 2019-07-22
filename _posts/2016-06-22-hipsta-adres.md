---
layout: post
title: Hipsta adres
type: post
categories:
- ".NET Developer"
tags:
- adres WWW
- IIS
- Visual Studio 2015
permalink: "/hipsta-adres/"
image: 'https://sienicki.dev/assets/tiles/hipstaadres.png'
category: 'blog' 
introduction: jak sprawić, aby adresy były piękne
---
Dzisiaj krótkie demo jak sprawić, aby adresy były piękne :smile:

Chodzi oczywiście o aplikacje odpalane i debugowane z Visual Studio. 
Domyślnie projekty Web chodzą na IIS Express i każda aplikacja WWW ma swój własny niepowtarzalny port.

Przykładowy adres http://localhost:3451 zastąpię http://artur.com :cool:

Trochę przy tym roboty i zastanawiam się jak daleko popłynęliśmy stosując takie podejście. 
Z jednej strony start projektu jest bardziej skomplikowany, 
a z drugiej każdy w zespole ma tak samo skonfigurowane środowisko + przyjemniejszy/łatwiejszy do wpisywania adres.

**[1]** Jeżeli nie macie to zainstalujcie sobie w systemie IIS'a (panel sterowania -> programy -> włącz/wyłącz funkcje windows).

![IIS instalacja]({{ site.baseurl }}/assets/images/IIS-install.png)

**[2]** Stwórzcie nową witrynę na IIS. Kontynuując przykład dla artur.com

![iis nowa witryna]({{ site.baseurl }}/assets/images/iis-nowa-witryna.png)

Tutaj mała uwaga, ścieżkę podaje do folderu projektu Web. Po dodaniu witryny powinno to wyglądać tak jak u mnie:

![zawartość aplikacji]({{ site.baseurl }}/assets/images/witryna-aplikacja.png)

**[3]** Upewnijcie się, że uprawnienia są właściwe dla:

- użytkownika puli aplikacji (tożsamość)
- użytkownika z dostępem do bazy danych
- użytkownika który ma dostęp do plików aplikacji (poświadczenia ścieżki fizycznej)

**[4]** Otwórzcie plik hosts (C:\Windows\System32\drivers\etc\hosts) i dodajcie do niego linię:  **127.0.0.1 artur.com**

+ zapiszcie (na 50% będzie to mikro problem) [wp-svg-icons icon="smiley" wrap="i"]

**[5]** Odpalcie projekt w Visual Studio jako administrator, przejdźcie do właściwości projektu i w zakładce Web zmieńcie IIS Express na Local IIS. W pole project url wprowadźcie http://artur.com i kliknijcie przycisk Create Virtual Directory.

![ustawienia projektu Visual Studio]({{ site.baseurl }}/assets/images/ustawienia-projektu-VS.png)

**[6]** Jeśli macie ustawione proxy to przyda się jeszcze dodać regułę wykluczającą. IE -> Opcje internetowe -> Połączenia -> Ustawienia sieci LAN -> Zaawansowane -> wykluczenie np. \*.artur.com

**[7]** Jeżeli używacie uwierzytelniania domenowego to należy jeszcze do rejestru (**HKEY\LOCAL\MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\MSV1\0**) 
dodać nową wartość **multi-string** o nazwie  **BackConnectionHostNames** i wartością waszego custom host (np. artur.com). 
[Więcej przeczytacie na stronach msu](http://support.microsoft.com/en-us/kb/896861 "Więcej przeczytacie na stronach msu").

**[8]** Cieszcie się pięknym i hipsterskim adresem :wink:
![artur.com-rezultat]({{ site.baseurl }}/assets/images/artur.com-rezultat.png)