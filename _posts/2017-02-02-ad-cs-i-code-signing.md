---
layout: post
title: AD CS i Code Signing
type: post
categories:
- ".NET Developer"
- Windows Server
tags:
- AD CS
- CA
- Code signing
permalink: "/ad-cs-i-code-signing/"
image: 'https://sienicki.dev/assets/tiles/hash.png'
category: 'blog' 
introduction: Zdobycie certyfikatu do podpisywania kodu.
---
Cel:

Zdobycie certyfikatu do podpisywania kodu.
Podpisany kod będzie działał tylko w domenie firmowej.

Jak można podejść do tematu?

1. Kupić sobie swój własny certyfikat programistyczny i nim podpisać stworzoną aplikację - głównym urzędem certyfikacji będzie verisign czy digicert - generalnie wymaganie zostanie spełnione. 
Cert jest poprawny i wszystko hula. 
Płaci się tylko wcale nie małe pieniądze - w tej chwili na rok można taki certyfikat wygenerować za 178$ (bez "Instant Reputation with Microsoft Smartscreen Filter"), a z 331$.
 Niby biedni nie jesteśmy, ale to i tak znaczna kwota.
2. Wygenerować certyfikat poprzez AD CS - ([Active Directory Certificate Services](http://social.technet.microsoft.com/wiki/contents/articles/1137.active-directory-certificate-services-ad-cs-introduction.aspx)).

Pochylę się dzisiaj i podzielę tą jakże tajemną wiedzą jak zdobyć certyfikat w domenie.

Zanim zaczniesz - oczywiście musisz mieć przygotowaną infrastrukturę - masz firmę - masz bizspark'a - pikuś, nie? coś wymyślisz :smile:
Potrzebujemy domeny oraz zainstalowanego i skonfigurowanego AD CS. [Instalacja i konfiguracja jest klikalna - z tym nie ma żadnych powazniejszych problemów](http://www.virtuallyboring.com/setup-microsoft-active-directory-certificate-services-ad-cs/).

[1] Generowanie kodu CSR. Czym jest kod CSR? 
Z ang. "żądanie podpisania certyfikatu" (Certificate Signing Request).

Jak się to robi?

Na swojej zwykłej stacji roboczej uruchom nowe zadanie (win+R), wpisz w okienko **mmc**. 
Dodaj/usuń przystawkę, wybierz certyfikaty->"Moje konto użyszkodnika" i ok.

Rozwijamy drzewo certyfikatów i przechodzimy do Osobisty/Certyfikaty.

Klikamy prawym myszy na certyfikaty, wybieramy "wszystkie zadania"->"Operacje zaawansowane"->"Utwórz żądanie niestandardowe..".

![]({{ site.baseurl }}/assets/images/CSR.png)

Pojawia się piękne okienko z&nbsp;opisem rejestracji certyfikatów, przeklikujemy.

![]({{ site.baseurl }}/assets/images/csr1.png)

Wybieramy zasady rejestracji certyfikatu na te z usługi Active Directory i przechodzimy dalej.  
 ![]({{ site.baseurl }}/assets/images/csr2.png)

Następnie wybieramy szablon żądania certyfikacyjnego. W moim przypadku chce certyfikat do podpisywania kodu - to wybieram nomen omen: "Podpisywanie kodu". Uwaga!

W tym miejscu dostępne są tylko standardowe szablony - i tak wybieramy - bo przykład dotyczy mojej domeny, ale zdarzyło mi się już wygenerować CSR z takiego szablonu - i został on odrzucony przez admin'a, bo on zdefiniował swoje własne szablony... w tym przypadku należy powiedzieć adminowi żeby sam sobie wygenerował CSR.
![]({{ site.baseurl }}/assets/images/csr3.png)
Po wybraniu szablonu możemy teraz zająć się właściwościami naszego CSR. Klikamy właściwości - zasad rejestracji usługi Active Directory.

![]({{ site.baseurl }}/assets/images/csr4.png)

Wypełniamy przyjazną nazwę + opis.

![]({{ site.baseurl }}/assets/images/csr5.png)

Wybieramy dane do certyfikatu. Ja wypełniłem&nbsp;prawie wszystko.

![]({{ site.baseurl }}/assets/images/CSR6.png)

Zaznaczamy, że chcemy eksportować klucz prywatny.

![]({{ site.baseurl }}/assets/images/csr7.png)

I zapisujemy do pliku.

![]({{ site.baseurl }}/assets/images/CSR8.png)

W widoku "przystawki" w żądaniach rejestracji certyfikatu powinniśmy zobaczyć nasz "pusty" certyfikat.

![]({{ site.baseurl }}/assets/images/csr9-1.png)

[2] Podpisanie CSR w CA (Certification Authority)

Uruchamiamy na serwerze CA. Prawym-> "All Tasks"->"Submit new request". Wybieramy CSR, który stworzyliśmy w kroku 1.

![]({{ site.baseurl }}/assets/images/CA1.png)

Po zaimportowaniu CSR'a, pojawi się on nam w folderze "Pending Request".

![]({{ site.baseurl }}/assets/images/ca2.png)

To co teraz musimy zrobić to right click na certyfikacie i wybieramy issue.

![]({{ site.baseurl }}/assets/images/CA3.png)

Nasz certyfikat pojawi się teraz w folderze Issued Certificates.

![]({{ site.baseurl }}/assets/images/CA4.png)

Zostało teraz tylko wyeksportować klucz publiczny. Można to zrobić np. przez dwuklik certyfikatu oraz kliknięciu "copy to file".

![]({{ site.baseurl }}/assets/images/ca5.png)

Wyeksportowany klucz przenosimy na maszynę na której wygenerowaliśmy&nbsp;CSR'a i instalujemy w lokalizacji w której podpowiada instalator.

Odświeżamy widok w mmc z certyfikatami. Mamy nowy podpisany certyfikat z eksportowalnym kluczem prywatnym. Teraz można go użyć np. do podpisania addin'a czy stworzenia certyfikatu w formacie snk do podpisywania bibliotek.

