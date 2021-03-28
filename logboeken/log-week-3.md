# Logboek
[terug](https://martijnmeeldijk.github.io/stage/)

## Week 3

### 22/03/2021

Vandaag eindigt onze eerste sprint. Normaal gezien zouden we een hele boel meetings hebben om te laten zien wat we hebben gemaakt. We zouden ook een 'retrospect' moeten doen. Dus, reflecteren over wat we hebben gedaan en wat beter kan. Spijtig genoeg heeft onze plaatsvervangende coördinator net vandaag verlof genomen. We weten dus niet echt wat we moeten doen. 

Ik zal dan maar verder werken aan mijn formulieren. 

De bedoeling is nu om een 'component' te maken voor een formulier die ik kan hergebruiken waar ik wil. Op dit moment zit ik vast. Ik zou graag iemand om hulp vragen, maar dat is op dit moment niet mogelijk.

Ik probeer verder.

Ik heb nu een klasse `InputComponent` die overerft van een abstracte klasse `InputBaseComponent`



Moest het iemand interesseren:

```typescript
import { Component, Input, Optional, Self, ChangeDetectorRef } from '@angular/core';
import { NgControl, FormControlName } from '@angular/forms';
import { InputBaseComponent } from '../input-base/input-base.component';

@Component({
  selector: 'actina-input',
  templateUrl: './input.component.html',
  styleUrls: ['./input.component.scss']
})
export class InputComponent extends InputBaseComponent {
  @Input() type = 'text';
  @Input() value: any = null;
  
  constructor(
    @Optional() @Self() ngControl: NgControl,
    @Optional() controlName: FormControlName,
    cdr: ChangeDetectorRef) {
    super(ngControl, controlName, cdr);
  }

  onValueChange(value: string) {
    this.value = value;
    this.onChange(value);
  }
  
  writeValue(value: any): void {
    if(value !== undefined || value !== '') {
      this.value = value;
    }
  }
}

```

```typescript
import { ChangeDetectorRef, Component, Input, OnInit, Optional, Self } from '@angular/core';
import { ControlValueAccessor, FormControl, FormControlName, NgControl } from '@angular/forms';

@Component({
  template: ''
})
export abstract class InputBaseComponent implements OnInit, ControlValueAccessor {
  @Input() class: string;
  @Input() label: string;
  @Input() formControlName: string;
  @Input() set required(required: boolean) { this._required = this.coerceBooleanProperty(required) };
  @Input() set allowClear(allowClear: boolean) { this._allowClear = this.coerceBooleanProperty(allowClear) };
  
  control: FormControl;
  _isDisabled = false;
  _required = false;
  _allowClear = false;
  protected onChange = (_: any) => { };
  protected onTouched = () => { };

  constructor(
    @Optional() @Self() private ngControl: NgControl,
    @Optional() private controlName: FormControlName,
    private cdr: ChangeDetectorRef) {
    if(this.ngControl != null) {
      this.ngControl.valueAccessor = this;
    }
  }

  ngOnInit(): void {
    this.control = this.controlName.control;
  }

  abstract writeValue(value: any): void;

  registerOnChange(fn: any): void {
    this.onChange = fn;
  }

  registerOnTouched(fn: any): void {
    this.onTouched = fn;
  }

  setDisabledState(isDisabled: boolean): void {
    this._isDisabled = isDisabled;
  }
  
  /** Coerces a data-bound value (typically a string) to a boolean. */
  coerceBooleanProperty(value: any): boolean {
    return value != null && `${value}` !== 'false';
  }
}

```

Ik ga heel eerlijk zijn, ik heb het grootste deel van de code overgenomen van een demo-project dat mijn coördinator mij heeft doorgestuurd. Het leek mij leuk om het op een gelijkaardige manier te implementeren, maar nu weet ik eerlijk gezegd niet meer wat ik moet doen. 

Morgen zal ik dan toch maar best wat hulp vragen. Ik probeer dit vandaag nog verder, maar als het niet lukt, dan lukt het niet...

### 23/03/2021

Oké dus vergeet die code die ik gisteren heb geschreven. Mijn coordinator zegt dat ik niet zo overdreven veel moet veralgemenen. Ik focus me om de belangrijkere dingen eerst in orde te krijgen. Vandaag herwerkt ik de services (de onderdelen ven het programma die praten met de API). Omdat Steven wat aanpassingen in de API had gedaan. 

### 24/03/2021

Vandaag maak ik een formulier dat ervoor zorgt dat een gebruiker zich kan inschrijven op een evenement, dit bouwt verder op de services die we gisteren hebben aangepast. Verder was er ook een grappige bug. Alle evenementen werden op elk vakje getoond. Dus als ik een evenement zou hebben op maandag en eentje op dinsdag, dan zouden er op maandag en dinsdag twee icoonjes verschijnen. Niet de bedoeling natuurlijk... 

### 25/03/2021

We hebben weer eens gebeld met Robin. Er werd ons medegedeeld dat we best de structuur van ons project wat herwerken. We hebben iets te veel rommel in het 'app' gedeelte (de code die de site opstart). We zouden deze code dus moeten verplaatsen naar de libraries (zoals ik voordien al heb uitgelegd). Verder heeft Robin ons een uitleg gegeven over lazy loading. 

> **Lazy loading** (also called on-demand loading) is an optimization technique for the online content, be it a website or a web app.
> Instead of loading the entire web page and rendering it to the user in one go as in bulk loading, the concept of lazy loading assists in loading only the required section and delays the remaining, until it is needed by the user.

[source](https://www.geeksforgeeks.org/what-is-lazy-loading/)

Het is dus een best practice die onze website sneller maakt. Ik ga er wel een hele boel dingen voor moeten aanpassen.

### 26/03/2021

Ik werk verder om lazy loading te implementeren.

Oké het is eindelijk klaar. Ik zat super lang vast aan één error. 