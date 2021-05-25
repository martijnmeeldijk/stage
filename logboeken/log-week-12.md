# Logboek
[terug](https://martijnmeeldijk.github.io/stage/)





## Week 12

### 25/05/2021

Vandaag mijn eerste dag alleen...

Na mijn meeting met Alexander ga ik direct aan de slag. Ik wil eerst en vooral ervoor zorgen dat alle teksten in de verschillende talen op onze website kloppen. 

Gemakkelijker gezegd dan gedaan. De code die zorgt voor de vertalingen werkt niet.

Na lang zoeken bleek het dus dat de code die checkt of de gebruiker een taal heeft gekozen niet werkt. Ik schrijf een domme fix.

```typescript
if(localStorage.getItem("language") != 'en' && localStorage.getItem("language") != 'fr'){
      localStorage.setItem("language", 'nl');
 }
```

Alvorens je klaagt: `localStorage.getItem("language") != null` werkte niet. Dit wel.



Nu ik klaar ben met dit gedeelte update ik mijn scripts om de *translation keys* uit alle delen van de app te halen. Ik vervang stukken tekst door keys waar nodig. Dan run ik mijn scripts opnieuw en **BAM**... Ik kan de vertalingen invullen in de gegenereerde JSON files.