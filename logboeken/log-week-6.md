# Logboek
[terug](https://martijnmeeldijk.github.io/stage/)





## Week 6

### 12/04/2021

Vandaag gaat het allemaal niet zo goed. Ik heb een beetje genoeg van al dat lockdown gedoe enzo (nou ja, wie niet). Het is echt moeilijk om me aan een taak te zetten. Alles lijkt een beetje voor niets. Ik ben nu al letterlijk 6 weken bezig aan een kalender. Ik zou het liefst de hele dag in foetushouding op de grond gaan liggen. 

Maarja, zo geraakt er ook geen werk gedaan.

We doen dan toch maar weer verder.

Vandaag hebben we een uitleg gekregen over  Azure B2C. Een systeem dat we blijkbaar gebruiken voor authenticatie. Mijn kennis hierin is beperkt, dus het is wel een uitdaging. 

Ik moet nu dus in de front-end een mechanisme maken dat ervoor zorgt dat als een user zich inlogt via dat ding van azure, hij ook op de kalender ingelogd is. Het token dat hij van azure krijgt moet dus ingelezen en gecheckt worden.

### 13/04/2021

Oké, Steven werk verder aan het ding van gisteren. Ik werk nu om de site meer responsive te maken. Zodat als je hem opent op je telefoon, hij ten minste bruikbaar is. Hiervoor moet ik wat css regeltjes en mediaqueries schrijven.



### 14/04/2020

Het authenticatie-gedeelte van de site werkt nog steeds niet. We hebben net een call gehad van langer dan een uur waar we het samen met onze coördinators probeerden op te lossen. Het is nog niet helemaal af. 

Ik heb ook nog wat verder gewerkt aan de 'responsiveness' van de site. 



### 15/04/2021

Ik heb volgens mij al in één van de vorige logs verteld dat ik een manier heb gevonden om de inhoud van de site te vertalen. Wel, dit werkt dus niet op de kalender zelf. De dagen, maanden en datums worden dus niet vertaald. Ik moet dus een manier zien te vinden om dat geregeld te krijgen. Bovendien wil ik ook (net als bij elke gewone site) dat je taalkeuze wordt opgeslagen. 

Om het eerst probleem op te lossen moest ik dus de lokalisatiedata die nodig was voor de kalender eerst ophalen. 

*in de module.ts*

```typescript
import localeNl from '@angular/common/locales/nl';
import localeFr from '@angular/common/locales/fr';
registerLocaleData(localeNl);
registerLocaleData(localeFr);
```

de kalender kan deze dan gebruiken.



 *in de calendar.component.ts*

```typescript
this.locale = localStorage.getItem("language");
```

in het html gedeelte moet je dan gewoon aan de kalender `[locale]="locale"` doorgeven.



### 16/04/2021

Oké, het is weer tijd voor iets nieuws. Alexander heeft mij gevraagd om een 'leden' gedeelte te maken op de applicatie. Hier zou je dus een lijst kunnen zien van de leden in de club. Je zou dan moeten kunnen zoeken naar een lid en zijn/haar contactgegevens opvragen.

Dit geheel zou dus in een aparte 'app' moeten komen. Volgens het principe van micro-frontends.

(fast-forward naar 16u)

Ik heb een hele tijd zitten zoeken. Nu heb ik een nieuwe app aangemaakt voor de leden, evenals een library die dan de meeste functionaliteit op zich zou moeten nemen. Er zijn wel wat problemen. Ik heb eigenlijk geen idee hoe ik er voor moet zorgen dat ik via de kalender op de ledenpagina kan raken. Ik heb nu de url (localhost:4201) gehardcoded in de html, maar dat kan niet de bedoeling zijn. Ik zal nog even zoeken, maar ik denk dat het een probleem voor maandag gaat zijn.