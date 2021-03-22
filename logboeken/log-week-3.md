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