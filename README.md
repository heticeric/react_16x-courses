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
- https://www.netlify.com/blog/2019/03/11/deep-dive-how-do-react-hooks-really-work/
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

## Reactive state with RxJS
### Avantages de la programmation reactive
[Pourquoi devrions nous tous utiliser la programmation réactive ?](reactive/why.md)

### Observable
La réactivité des `Observable` peut également nous intéresser dans la gestion des états applicatif.
[Petite révision sur l'Observable RxJS](reactive/README.md)

### Observable in depth
[Plongée dans le code des `Observable`](reactive/indepth.md).

### Subject
[La classe `Subject` étend `Observable`](reactive/subject.md).

### Tips & tricks
[Quelques exemples intéressant d'usage des streams reactifs](reactive/tips.md).

### Gestion optimisée des états applicatifs
[Est-il possible de s'approprier des streams RxJS pour gérer les `state` d'une application *react* ?](reactive/store.md)