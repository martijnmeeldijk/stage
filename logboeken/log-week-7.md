# Logboek
[terug](https://martijnmeeldijk.github.io/stage/)





## Week 6

### 19/04/2021

Er wordt van mij gevraagd om een nieuw onderdeel van de app te schrijven. Het **members** gedeelte. Hier zouden leden van de club elkaar kunnen opzoeken, hun eigen profiel bekijken en bewerken, ... . De bedoeling is dus dat dit gedeelte in een aparte '*microfrontend*' wordt gestoken, evenals de kalender en eventuele andere onderdelen. Zo zouden de onderdelen onafhankelijk van elkaar onderhouden kunnen worden. 

Ik ga aan de slag met het maken van de componenten voor het members gedeelte. Ik genereer de applicatie, ... -- enzovoort --

### 20/04/2021

Nu ik een paar componenten klaar heb, kan ik beginnen met de implementatie van de micro-frontends. 

Ik heb een goeie uitleg gevonden: 

https://onna.dev/implement-micro-frontends-in-angular-using-nx/



### 21/04/2021

Wat ik gisteren heb geschreven is dus niet zo gemakkelijk als het lijkt. Ik krijg het maar niet aan de praat.

Het zou moeten werken...



### 22/04/2021

Op dit moment heb ik dus drie applicaties:

`Dashboard` ,`Calendar` en `Members`. `Dashboard` is het *entry-point* van onze app. Het bevat de header, navigatie, vertalingen, ... . Als je de vorige 6 weken van mijn logboek hebt gelezen, ken je `calendar` hopelijk al. `Members` is nu het nieuwe gedeelte.

We kunnen dus nu d.m.v. `webpack 5 module federation` ervoor zorgen dat deze 3 applicaties gescheiden, toch samen kunnen werken. 

Als we er nu bijvoorbeeld voor willen zorgen dat onze `calendar` kan worden ingeladen in het `dashboard`, dan steken we volgende code in de `webpack.config.js` van onze `calendar`:

```javascript
const ModuleFederationPlugin = require("webpack/lib/container/ModuleFederationPlugin");
const mf = require("@angular-architects/module-federation/webpack");
const path = require("path");

const sharedMappings = new mf.SharedMappings();
sharedMappings.register(
  path.join(__dirname, '../../tsconfig.base.json'),
  [/* mapped paths to share */]);

module.exports = {
  output: {
    uniqueName: "calendar"
  },
  optimization: {
    // Only needed to bypass a temporary bug
    runtimeChunk: false
  },
  plugins: [
    new ModuleFederationPlugin({
      
        // For remotes (please adjust)
        name: "calendar",
        filename: "remoteEntry.js",
        exposes: {
            './Module': './apps/calendar/src/app/app.module.ts', // deze lijn 
        },        

        shared: {
          "@angular/core": { singleton: true, strictVersion: true }, 
          "@angular/common": { singleton: true, strictVersion: false }, 
          "@angular/router": { singleton: true, strictVersion: true },

          ...sharedMappings.getDescriptors()
        }
        
    }),
    sharedMappings.getPlugin(),
  ],
};

```

de lijn `exposes: {'./Module': './apps/calendar/src/app/app.module.ts'}` is zeer belangrijk, want deze vertelt welk deel van onze app er blootgesteld wordt aan de buitenwereld.



In onze `dashboard` zorgen we er nu voor dat de router weet dat `calendar` bestaat:

```typescript
const routes: Route[] = [
  {
    path: '',
    component: BaseComponent,
    children: [
      {
        path: 'calendar',
        loadChildren: () =>
          loadRemoteModule({
            remoteEntry: 'http://localhost:4201/remoteEntry.js',
            remoteName: 'calendar',
            exposedModule: './Module',
          }).then((m) => m.AppModule),
      }
    ]
    
  },
];
```

We vertellen hem ook dat hij moet zoeken naar `AppModule`, zoals in het vorige stuk code werd aangeduid.



### 23/04/2021

Wat ik dus gisteren neer heb geschreven werkt niet en ik weet niet waarom. Ik maak een mini testproject om te zien of ik het dan wel aan de praat krijg.

Nope.

Oké, ik kopieer een project van iemand anders, gooi het in een git repository, commit, en overschrijf het met dat van mij. Nu kan ik gemakkelijk zien wat de verschillen zijn.

Oh nee

Oké we gebruikten dus blijkbaar van bepaalde dependencies de verkeerde versies. Geweldig.