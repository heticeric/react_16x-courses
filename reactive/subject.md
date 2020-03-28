# Subject
Un `Subject` est un `Observable` qui dispose en plus des méthodes `next()`, `error()` et `complete()` de l'`Observer`.
Cela lui donne la possibilité d'émettre lui-même des valeurs complémentaires à ses abonnés.

## Multicast vs unicast
Mais surtout, un `Subject` diffuse son stream en **multicast**.
A l'opposé, les `Observable` diffusent les valuers en **unicast**. 

> Unicast exemple

```js
import * as Rx from "rxjs";

const observable$ = Rx.Observable.create
(
	observer => observer.next( Math.random() )
);

// One subscription gest some values
observable$.subscribe
(
	data => console.log(data) // 0.666 (random)
);

// While another subscription gest others
observable.subscribe
(
	data => console.log(data); // 0.0042 (random)
);
```

Multicast signifie que l'**exécution est partagée entre tous les abonnés du `Subject`**.
Tel un `event emitter`, le `Subject` maintient une liste d'observateurs. Ainsi, lors d'un nouvel abonnement, l'`Observer` est simplement enregistré dans la liste.

> Multicast exemple
 
```js
import * as Rx from "rxjs";

const subject$ = new Rx.Subject();

// One subscriber
subject$.subscribe(
	data =>  console.log( data ) // 0.42 (random)
});

// Another subscriber gets the same data
subject$.subscribe(
	data =>  console.log( data ) // 0.42 (random)
});

// Both will receive the same values
subject$.next( Math.random() );
```

## Variantes de `Subject`
### BehaviorSubject
Le `BehaviorSubject` a comme caractéristique de stocker sa dernière valeur.
Ainsi, quelque soit le moment de la souscription, il est toujours possible de récuprérer la valeur en cours d'usage.

> Accéder à la dernière valeur d'un stream

```js
import * as Rx from "rxjs";

const subject$ = new Rx.BehaviorSubject();

subject$.subscribe
(
	data => console.log( `Subscriber A: ${ data }` );
);

subject$.next( Math.random() );
subject$.next( Math.random() );

subject$.subscribe
(	
	data => console.log( `Subscriber B: ${ data }` );
);

subject$.next( Math.random() );
console.log( subject$.value ); //<--- Last value

// Subscriber A: 0.24957144215097515
// Subscriber A: 0.8751123892486292
// Subscriber B: 0.8751123892486292
// Subscriber A: 0.1901322109907977
// Subscriber B: 0.1901322109907977
// 0.1901322109907977
```

**Note** `BehaviorSubject` peut également recevoir sa valeur par défaut en argument de son constructeur.

### ReplaySubject
En plus du `BehaviorSubject`, le `ReplaySubject` permet de stocker plusieurs valeurs, et de circonscrire leur temps de conservation.

```js
import * as Rx from "rxjs";

// keep the 2 last
const subject$ = new Rx.ReplaySubject( 2, /* optional time to keep values in ms */ );values

subject$.subscribe
(
	data => console.log( `Subscriber A: ${ data }` );
);

subject$.next( Math.random() )
subject$.next( Math.random() )
subject$.next( Math.random() )

subject$.subscribe
(
	data => console.log( `Subscriber B: ${ data }` );
);

subject$.next( Math.random() );

// Subscriber A: 0.3541746356538569
// Subscriber A: 0.12137498878080955
// Subscriber A: 0.531935186034298
// Subscriber B: 0.12137498878080955
// Subscriber B: 0.531935186034298
// Subscriber A: 0.6664809293975393
// Subscriber B: 0.6664809293975393
```

### AsyncSubject
Ici, les observateurs ne recevront que la dernière valeur du stream, une fois celui-ci clôturé par `complete()`.

```js
import * as Rx from "rxjs";

const subject$ = new Rx.AsyncSubject();

subject$.subscribe
(
    data => console.log( `Subscriber A: ${ data }` );
);

subject$.next( Math.random() )
subject$.next( Math.random() )
subject$.next( Math.random() )

subject$.subscribe
(
    data => console.log( `Subscriber B: ${ data }` );
);

subject.next( Math.random() );
subject.complete();//<--- Here comes the last emission of values

// Subscriber A: 0.4447275989704571
// Subscriber B: 0.4447275989704571
```

### Références
- https://medium.com/@luukgruijs/understanding-rxjs-subjects-339428a1815b
- https://medium.com/@luukgruijs/understanding-rxjs-behaviorsubject-replaysubject-and-asyncsubject-8cc061f1cfc0