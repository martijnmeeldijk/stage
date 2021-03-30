# Logboek
[terug](https://martijnmeeldijk.github.io/stage/)





## Week 4

### 29/03/2021

Vandaag heb ik veel gewerkt aan de styling van de applicatie. Ik heb een navigatiebalk gemaakt met het logo van het bedrijf. Ik heb ook samen met steven wat bug uit de applicatie gehaald. 

Een nieuwe feature! De snackbar! (nee niet zo een snackbar)

> A **Snackbar** is a message used to give users feedback after taking an action. They are ephemeral and time out after a few seconds. Use **Snackbars** when you need a quick, lightweight way of providing feedback following an action they've taken.

Een snackbar is dus eigenlijk een soort van pop-up notificatie. 

Ik heb lang zitten prutsen omdat mijn snackbar altijd achter onze pop-up vensters terecht kwam. De oplossing was om de z-index van onze pop-ups te veranderen zodat ze niet voor de snackbar terecht kwamen.

```scss
.modal{
  margin-top: 64px;
  z-index: 999 !important;
}
.modal-backdrop{
  z-index: 998 !important;
}
```



### 30/03/2021

Oké dus de bedoeling is om wanneer een error zich voordoet, deze te tonen in de snackbar. Nu dus in alle formulieren gaan instellen dat de snackbar moet getoond worden bij een fout (en ook als het formulier geslaagd is).



Ik heb nu ook een sidebar toegevoegd en de bestaande code wat opgeschoond.

![image-20210330172227829](img/log-week-4/image-20210330172227829.png)

Het begint er als een echte applicatie uit te zien.



Nu doe ik als laatste vandaag nog wat styling voor het scherm waar je de inschrijvingen voor een evenement kan zien.