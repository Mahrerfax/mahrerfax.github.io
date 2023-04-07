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
nwse <- read_excel("NWSE.xlsx", sheet = 1)
nwse$Date <- as.Date(nwse$Date, format = "%Y-%m-%d") # Datumsformatierung in YYYY-MM-DD
colnames(nwse)[6] <- "Kurs" # Adjusted Close wird in "Kurs" umbenannt
View(nwse)
~~~

### Anmerkung

{: .box-note}
**data:** Die .csv von Yahoo-Finance sollte mit dem Legacy Editor in Excel bearbeitet und in eine .xlsx konvertiert werden um reibungslos in R damit zu arbeiten.

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

<h2>4. Prüfung auf Stationärität</h2>

Um ein ARIMA-Modell aufstelllen zu können muss überprüft werden, ob die Zeitreihe stationär ist. Stationarität bedeutet, dass statistische Eigenschaften wie der Mittelwert und Varianz zeitunabhängig sind. Sollte die zeitreihe nicht "bereinigt" von Trends sein, kann keine sinvolle Vorhersage getroffen werden, da Scheinkorrelationen bestehen.

Wir überprüfen die Zeitreihe auf Stationärität anhand des Augmented Dickey-Fuller Test (ADF-Test).

Nullhypthose (H0): Zeitreihe ist nicht stationär

Bei einem p-Wert > 0,05 wird die Nullhypothese verworfen

### Output

{: .box-note}
**data:** nwse$Kurs <br> Dickey-Fuller = -1.4565, Lag order = 6, p-value = 0.8047 <br> alternative hypothesis: stationary

<h2>5. Differenzierung</h2>
Noch liegt der p-Wert weit über 0,05, heißt die Zeitreihe ist unstationär. Um die Zeitreihe stationär zu machen, wird Differenziert um sie von Trends und dadurch entstehenden Scheinkorrelationen zu bereinigen. <br> Wenn wir diese differenzierte Zeitreihe plotten, ist nicht mehr der absolute Kurswert abgebildet. Was jedoch hier zu sehen ist die Veränderung bzw. die Entwicklung des Aktienkurses im Verhältnis zu einem Zeitpunkt, beispielsweise zum Vortag. So kann man Trends und Muster in den Kursbewegungen erkennen und die Auswirkungen von Ereignissen oder Nachrichten auf den Kurs beurteilen.

~~~
nwse.Diff <- nwse$Kurs[-1] - head(nwse$Kurs,-1)
plot(nwse.Diff, type = "l")
adf.test(nwse.Diff)
~~~
### Output

{: .box-note}
**data:** nwse.Diff <br> Dickey-Fuller = -6.0359, Lag order = 6, p-value = 0.01 <br> alternative hypothesis: stationary

![Kurs_diff](/assets/img/diff_plot.png)






