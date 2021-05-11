# Logboek
[terug](https://martijnmeeldijk.github.io/stage/)





## Week 10

### 10/05/2021

 Het leek mij leuk om vandaag eens te proberen met behulp van de adviezen van mijn mentoren en bug op te lossen. Dit bleek uiteindelijk niet zo gemakkelijk.

De bug zit in het gedeelte van de code waar de evenementen uit de API worden opgehaald. Ze worden namelijk niet direct ingeladen als de app start, maar pas als er een interactie met de gebruiker gebeurt.

Eén van mijn mentoren had een tijd geleden een stukje code geschreven dat dit zou kunnen oplossen.

```typescript
getAllEvents(): Observable<CalendarEvent[]> {
    return this.http.get<IEvent[]>(baseUrl, { responseType: 'json' }).pipe(map(res => res.map(event => ({
      id: event.id.toString(),
      title: event.title,
      start: new Date(event.startDate),
      end: new Date(event.endDate),
      cssClass: event.eventIcon
    }))));
  }
```

Op een oude versie van onze app zou dit gewerkt hebben, spijtig genoeg nu niet meer. Dit komt doordat het icoontje van het evenement nu niet meer wordt bijgehouden in het evenement, maar in het evenementtype. 

Ik had dat dan zo opgelost:

```typescript
calendarEvents : CalendarEvent[] = []
  
  getAllEvents(): CalendarEvent[] {
  this.http.get<IEvent[]>(baseUrl, { responseType: 'json' }).subscribe( res => {
    res.forEach(element => {
      this.eventTypeService.get(element.eventTypeId).subscribe (eventType => 
       this.calendarEvents.push({
         id : element.id.toString(),
         title : element.title,
         start : new Date(element.startDate),
         end : new Date(element.endDate),
         cssClass : eventType.icon,
       }) )
    });
  })
    return this.calendarEvents
}
```

Maar dan zitten we weer met onze bug. Goed, ik ga aan iets anders werken en misschien komt het antwoord later ineens via een soort briljante inval tot mij.



### 11/05/2021

