# RxJS Observable
## Observable
1. `Observables` sont les entités qui émettent des valueurs lorsqu'une modification apparaît.

1. `Observers` sont les entités qui reçoivent les données émises par les `Observables`.

L'émission des données aux `Observers` est dit **PUSH** (émission depuis la source de données, lorsque l'une d'elle est disponible).

Contrairement aux appels de fonctions traditionnels, qui sont issus par **PULL** (demande de celui qui souhaite la données.

## Composants RxJS
- `Observables` — éléments qui possèdent et envoient les modifications aux `Observers`.
- `Observers` — éléments qui écoutent les modifications de valeur soumises par les `Observables`.
- `Operators` — **fonctions pures** qui gèrent les valeurs des collections ; e.g map, filter, concat, reduce, etc.
- `Subjects` — *event emitters* qui broadcastent un flux de données à de multiple `Observers`.
- `Schedulers` — *dispatchers* centralisés pour controler la concurrence de valeurs. Permettent une coordination lorsqu'un calcul se présente.

## Implémentation

> Mixer des valeurs synchrones et asynchrones 

```js
const observable$ = new Rx.Observable
(
	subscriber =>
	{
	  subscriber.next(1);
	  subscriber.next(2);
	  setTimeout
	  (
	  	() =>
	  	{
		    subscriber.next(3);
		    subscriber.complete();
	  	}, 1000
	  );
	}
);
```

> Pour recevoir le flux de valeurs
 
```js
observable$.subscribe( val => console.log( val ) );
```

ou plus simple

```js
observable$.subscribe( console.log );
```

> Il est également possible de suivre les erreurs du flux de l'`Observable`

```js
observable$.subscribe
(
	{
	  next(x) { console.log(x); /* New value emitted */ }
	  ,error(err){ console.error(err); /* Error in stream */ },
	  complete(){ console.log("done"); /* Stream is closed and completed */ }
	}
);
```

## Auto disposable

**Attention**, l'`Observable` **se clotûre automatiquement dès que toutes ses valeurs sont émises**.

```js
import { from } from "rxjs"

const a$ = from( [1, 5, 9] )

a$.pipe(
  map( v => v *3 )
).subscribe(
  {
    next( v ){ console.log( `A ${v}` ) }
    ,complete(){ console.log( `I'm done !` ); }
  }
);
```

## Annuler une souscription
Au moment de l'abonnement à un stream, nous pouvons récupérer une référence pour stopper la souscription.

> unsubcribe() handler

```js
const observable$ = new Observable
(
 subscriber =>
 {
 	const intervalId 
 		= setInterval
 		(
 			() => subscriber.next("hi");
  			, 1000
  		);
  	
  	// Here we delare a function reference to unsubscribe to the current stream
  	return function unsubscribe()
  	{
  		clearInterval(intervalId);
 	};
});

const subscription = observable$.subscribe( x => console.log(x) );

setTimeout
(
	() => subscription.unsubscribe();//<--- here
	, 3000
);
```

#### Références
- https://levelup.gitconnected.com/introduction-to-reactive-programming-with-rxjs-114fa5e57eb1

### Tips & tricks

#### Références
- http://slides.com/michalzalecki/reactive-javascript