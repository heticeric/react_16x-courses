# Advanced course for React 16.x

## Hooks
### Introduction
La version 16.8 permet d'utiliser des composants fonctionnels `statefull`.
Cette emploi passe par fonctions, les [Hooks](hooks/README.md).
Auparavant, seules les composants héritant de la classe `React.Component` permettaient de gérer des états.

### Guide
Étape par étape, suivez la mise en place d'un [projet react](https://github.com/heticeric/react_16x-demo)

## Custom hooks
Vous pouvez créer votre propres hooks, simplement en faisant une fonction.

```js
import React, { useState } from "react";

const useToggle = (initialValue) =>
{
  const [ toggleValue, setToggleValue ] = useState(initialValue);
  const toggler = () => setToggleValue( !toggleValue );

  return [ toggleValue, toggler ];
}

export default useToggle;
```

Ainsi, vous pourrez réutiliser votre code entre pusieurs composants.
Imaginons par exemple un hook personnalisé pour s'authentifier à une API.

## Optimisation
Certains hooks permettent d'éviter aux composants des rendus inutiles.
La technique utilisée se nomme [mémoïsation](hooks/memoization.md). Elle consiste à mémoriser certaines valeurs et les utiliser telle une mise en cache.

## Hook hook hook

- [Brightleaf React Hooks](https://brightleaf.dev/hooks/)
- [Awesome React Hooks Resources](https://github.com/rehooks/awesome-react-hooks)
- [Collection of React Hooks](https://nikgraf.github.io/react-hooks/)
- [react-fetching-library](https://github.com/marcin-piela/react-fetching-library)

### In-depth

#### Références
- https://medium.com/@WebReflection/demystifying-hooks-f55ad885609f

## Optimisations 
### Inventaire des méthodes de gestion des états
#### Références
- https://www.robinwieruch.de/react-state

### Colocation des états
Ken C. Dodds suggère de laisser les états applicatifs aussi proches que possible du composant qui les utilisent.
[Il nomme cette méthodologie *co-location*](optim/README.md)

### Autres méthodes 
#### Références
- https://www.toptal.com/react/optimizing-react-performance
- https://medium.com/better-programming/performance-optimization-with-react-hooks-and-memo-e3186f7ff9ab
- https://dev.to/oahehc/few-tips-to-optimizing-performance-of-react-project-5h25
  
## Context API
### Introduction
La nouvelle version [Context API](context/README.md) du framework, mis à jour récemment, simplifie l'échange de `props` dans une hiérarchie de composants à plusieurs niveaux. 

### `useContext` hook
Obtenir les données d'un contexte `Provider` parent

```js
import React, { useContext } from 'react'

const CurrentRoute = React.createContext({ path: '/welcome' })
const CurrentUser = React.createContext(undefined)
const IsStatic = React.createContext(false)

export default () =>
{
  let currentRoute = useContext(CurrentRoute)
  let currentUser = useContext(CurrentUser)
  let isStatic = useContext(IsStatic)

  return (
    !isStatic &&
    currentRoute.path === '/welcome' &&
    (currentUser
      ? `Welcome back, ${currentUser.name}!`
      : 'Welcome!'
    )
  )
}
```

### Question performances
La tentation est forte de vouloir remplacer le framework [Redux](https://redux.js.org/) par un `Context` global, faisant œuvre de *store*.
[Quid des contraintes et performances ?](context/performances.md)

### Multi contexts to the rescue
[Motif de conception pour de larges applications scalables](context/context_wise.md)

### Bonnes pratiques
[Best practices](context/best_practices.md)

## Observable RxJS
### Observable

#### Références
- https://levelup.gitconnected.com/introduction-to-reactive-programming-with-rxjs-114fa5e57eb1

### Tips & tricks

#### Références
- http://slides.com/michalzalecki/reactive-javascript

### Subject

#### Références
- https://medium.com/@luukgruijs/understanding-rxjs-subjects-339428a1815b
- https://www.akveo.com/blog/understanding-observables-and-subjects-in-rxjs-multicasting-values

### Gestion optimisée des états applicatifs
#### Références
- https://blog.logrocket.com/rxjs-with-react-hooks-for-state-management/ 
- https://hackernoon.com/diy-redux-with-rxjs-rxdx-23163a87ade1
- https://blog.betomorrow.com/replacing-redux-with-observables-and-react-hooks-acdbbaf5ba80

#### Libraries
- https://github.com/DanWahlin/Observable-Store
- https://meiosis.js.org/