#Tips and tricks

## Plus-values de la programmation reactive
- Manipulation de données
- Gestion des events 
- Gestion des erreurs
- Callback hell
- Promise hell
- Données reçues en web workers
- Gestion des mise à jour de données
- Concurrence
- Race condition
- Gestion de la complexité des états applicatifs
- Dépendances entre états, et *computed properties*
- Fuite de mémoire
- Effets de bord

## Buffer
> Exemple avec mise en buffer 

```js
const output = document.getElementById('output');  
const button = document.getElementById('b');

fromEvent( button, 'click' )
  .pipe
  (
    bufferCount(3) // <-- collects (buffers) 3 events and then emits 1 event with an array of buffered events.
  ).subscribe
  (
      v => output.innerHTML = v
  );
```

[@see it live](https://codepen.io/heticschool/pen/yLNGZgo)

> Exemple avec laps temporel 

```js
const output = document.getElementById('output');  
const button = document.getElementById('b');

const click$ = fromEvent( button, 'click' );

click$
  .pipe
  (
    /* buffers all clicks until the function inside emits something */
    bufferWhen( _ => click$.pipe( delay( 400 ) )/* The function inside emits its first event 400ms after the click */ )
    ,filter( events => events.length <= 3 )
  ).subscribe
  (
      v => output.innerHTML = v
  );
```
[@see it live](https://codepen.io/heticschool/pen/eYNbxVr)

### Références
- https://x-team.com/blog/rxjs-observables/