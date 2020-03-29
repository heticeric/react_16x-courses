# RxJS store for React

## Demo time
Nous allons nous servir de la classe [`BehaviorSubject`](subject.md#BehaviorSubject).

`BehaviorSubject`

1. Permet le **multicasting**
1. Stocke la dernière valeur émise aux `Observers`.

De plus, nous allons encapsuler la manipulation des valeurs de ce *stream* dans une API, souvent nommé **service** dans les architectures. Il agit tel une [`Facade`](https://en.wikipedia.org/wiki/Facade_pattern), c'est-à-dire qu'il masque l'implémentation du `BehaviorSubject` et donne accès à des méthodes pour le manipuler de l'exterieur.

Ainsi, les données sont **protégées**, **en lecture seule** et **immutables**.

[React RxJS store example](https://github.com/heticeric/react-rxjs_store-demo)

### Références
- https://blog.bitsrc.io/sharing-data-between-react-components-using-rxjs-922a46c13dbf
- https://blog.logrocket.com/rxjs-with-react-hooks-for-state-management/
- https://hackernoon.com/diy-redux-with-rxjs-rxdx-23163a87ade1
- https://blog.betomorrow.com/replacing-redux-with-observables-and-react-hooks-acdbbaf5ba80
- https://blog.soshace.com/react-hooks-rxjs-or-how-react-is-meant-to-be/
- https://jasonwatmore.com/post/2019/02/13/react-rxjs-communicating-between-components-with-observable-subject

### Libraries
- [Akita](https://netbasal.gitbook.io/akita/)
	- https://engineering.datorama.com/oop-and-rxjs-managing-state-in-react-with-akita-de981e09307
	- https://medium.com/@thomasburlesonIA/https-medium-com-thomasburlesonia-react-hooks-rxjs-facades-4e116330bbe1
- https://github.com/DanWahlin/Observable-Store
- https://meiosis.js.org/