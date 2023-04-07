---
layout: post
title: Durchführung einer ARIMA-Analyse mit R
subtitle: Eine einfache Schritt für Schritt Anleitung
cover-img: /assets/img/stock-gdeec8ab8f_1920.jpg
thumbnail-img: /assets/img/thumb.png
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
**Error:** Error in library(XXXX) : es gibt kein Paket namens ‘XXXX’

Kannst Du hiermit das fehlende Package installieren:
~~~
install.packages("XXXX")
~~~
