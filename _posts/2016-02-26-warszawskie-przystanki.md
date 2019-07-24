---
layout: post
title: Warszawskie przystanki
type: post
categories:
- SQL Server
tags:
- przystanki
permalink: "/warszawskie-przystanki/"
image: 'https://sienicki.dev/assets/tiles/548116_525298757487759_302758926_na.jpg'
category: 'blog' 
introduction: Baza danych dla chętnych
---
Ostatnio przy obiedzie usłyszałem dość oryginalne pytanie które padło podczas rozmowy kwalifikacyjnej.

_**Ile jest przystanków w Warszawie?**_

Stwierdziłem, że mam strasznie zrytą banię, bo w 2013 roku jak tworzyłem aplikację " **No to jazda**" miałem okazję parsować dane z strony ZTM - było ich wtedy 5260... :grinning:

Mniej więcej bym odpowiedział, ale zakładam, że rekrutujący tym pytaniem chciał zmusić kandydata do czegoś innego... zobaczyć na żywo jak kombinuje z xxxx wymaganiami biznesowymi? :stuck_out_tongue_winking_eye:

Nieważne... w każdym razie - stwierdziłem, że projekt już dawno zabiłem, a dane może komuś się przydadzą do jakiegoś testowego projektu - udostępniam :simple_smile:

Schemat:
[![schemat-NTJ]({{ site.baseurl }}/assets/images/schemat-NTJ.png)]({{site.baseurl}}/assets/images/schemat-NTJ.png)

SQL schematu:

```sql
CREATE TABLE [dbo].[Miejscowosc]
([Id] [int] NOT NULL, 
[Nazwa] [nvarchar](50) NULL, 
[MiastoId] [int] NULL, CONSTRAINT [PK_NTJ_Miejscowosc] 
PRIMARY KEY CLUSTERED ( [Id] ASC )WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY] ) ON [PRIMARY] 

CREATE TABLE [dbo].[Przystanek]( 
  [Nazwa] [nvarchar](50) NOT NULL, 
  [MiejscowoscId] [int] NULL, 
  [WspolrzedneGPS] [geography] NULL, CONSTRAINT [PK_NTJ_Przystanek] PRIMARY KEY CLUSTERED ( [Nazwa] ASC )WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY] ) ON [PRIMARY] TEXTIMAGE_ON [PRIMARY] 
ALTER TABLE [dbo].[Miejscowosc] WITH CHECK ADD CONSTRAINT [FK_NTJ_Miejscowosc_NTJ_Miasto] FOREIGN KEY([MiastoId]) REFERENCES [dbo].[Miasto] ([Id]) GO ALTER TABLE [dbo].[Miejscowosc] CHECK CONSTRAINT [FK_NTJ_Miejscowosc_NTJ_Miasto] GO ALTER TABLE [dbo].[Przystanek] WITH CHECK ADD CONSTRAINT [FK_NTJ_Przystanek_NTJ_Miejscowosc] FOREIGN KEY([MiejscowoscId]) REFERENCES [dbo].[Miejscowosc] ([Id]) GO ALTER TABLE [dbo].[Przystanek] CHECK CONSTRAINT [FK_NTJ_Przystanek_NTJ_Miejscowosc] GO
```

[Pobierz skrypt z danymi - "Przystanki Warszawy.sql".]({{site.baseurl}}/assets/archives/Przystanki Warszawy.sql)

