---
layout: post
title: Durchführung einer ARIMA-Analyse mit R
subtitle: Eine einfache Schritt für Schritt Anleitung
cover-img: /assets/img/stock-gdeec8ab8f_1920.jpg
thumbnail-img: /assets/img/thumbnail_ARIMA.png
share-img: /assets/img/stock-gdeec8ab8f_1920.jpg
tags: [R, ARIMA,]
---
<h1>Was ist überhaupt ein ARIMA-Modell?</h1>
Das ARIMA-Modell ist ein statistisches Modell, das zur Vorhersage von Zeitreihen verwendet wird. Es besteht aus drei Elementen: AutoRegressive (AR), Integrated (I) und Moving Average (MA). Der AR-Teil zeigt die Beziehung zwischen aktuellen Zeitreihenwerten und früheren Werten, der I-Teil beschreibt die Verarbeitung nicht-stationärer Prozesse und der MA-Teil zeigt die Abhängigkeit aktueller Werte von unvorhersehbaren Ereignissen in der Vergangenheit.
Anhand eines vereinfachten Beispieles werde ich in diesem Post demonstrieren, wie man solch eine Analyse in R durchführt
<h2>1. Importieren der benötigten Modelle</h2>
Für das Aufstellen eines ARIMA-Modells benötigen wir folgende R-Packages:

~~~
library(dplyr)
library(forecast)
library(tseries)
library(readxl)
library(ggplot2)
library(ggthemes)
~~~

Solltest Du folgende Fehlermeldung bekommen:
### Error
{: .box-error}
Error in library(XXXX) : es gibt kein Paket namens ‘XXXX’

Kannst Du hiermit das fehlende Package installieren:
~~~
install.packages("XXXX")
~~~

<h2>2. Laden des Datensatzes</h2>
Als Beispiel-Daten benutzen wir die Aktienkurse vom 02.02.2022 - 02.02.2023 der NEW WORK SE. Die Aktienkurs-Daten kann man sich ganz einfach auf https://de.finance.yahoo.com/ beschaffen und anschließend in R importieren.

~~~
#nwse <- read.csv("NWSE.csv", header = TRUE) # Falls .csv benötigt wird
nwse <- read_excel("937498.xlsx", sheet = 1)
nwse$Date <- as.Date(nwse$Date, format = "%Y-%m-%d") # Datumsformatierung in YYYY-MM-DD
colnames(nwse)[6] <- "Kurs" # Adjusted Close wird in "Kurs" umbenannt
View(nwse)
~~~

<h2>3. Visualisierung des Datensatzes</h2>
Um einen anfänglichen Überblick über die Daten zu bekommen, sollten diese visualiert werden. In R ist das sehr simpel und schnell gemacht mit Hilfe der Packages "ggplot2" und "ggthemes".

~~~
chart_kurs <-ggplot(nwse, aes(x = Date, y = Kurs))+ 
  geom_line()+
  scale_x_date("Feb '22 - Feb '23")+
  ylab("Kurs")+
  theme_solarized_2()

chart_kurs
~~~


![Kurs](/assets/img/Rplot.png)
