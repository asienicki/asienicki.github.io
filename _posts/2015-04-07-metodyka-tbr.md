---
layout: post
title: Metodyka TBR
type: post
categories:
- Dobre praktyki
tags:
- komentarze w TFS
- metodyka
- nazewnictwo
- TBR
permalink: "/metodyka-tbr/"
image: 'https://sienicki.dev/assets/tiles/sowy.jpg'
category: 'blog' 
introduction: Leniwe opisywanie wiadomości w commitach
---
Metodyka **TBR** - jest to sposób nazywania komentarzy przy każdym „checkin/commit”.

Spośród rodzajów commit’ów możemy wydzielić kilka akcji:
1. praca dotycząca zadania (**T**  - task)
2. rozwiązanie błędu (**B**  - bug)
3. wykonanie funkcjonalności nie uwzględnionej jeszcze w zadaniach (**F**  -  feature)
4. refaktoryzacja kodu (**R** - refactor)
  - formatowanie (**F** - format)
  - ulepszenie mechanizmu działania ( **I** - improve)
  - przenoszenie (**M** - move)
  - optymalizacja (**O** - optimize)

Głównym założeniem metodyki  **TBR**  jest uproszczenie przeglądania i analizowania kodu. Programiści przez wymuszenie metodyki  **TBR**  robią mniejsze i łatwiejsze do przeanalizowania commit-y/checkin-y.

Przykłady wykorzystania metodyki TBR:

**T -> Dodanie przycisku wyślij maila na zakładce dialogi.**
**B -> Naprawienie skryptu js dla IE**
**R -> (****FIMO)**
**R -> (F)**
**R -> (MO)**

Przy refaktoryzacji (R) dopuszczalne jest niestosowanie komentarzy tekstowych.