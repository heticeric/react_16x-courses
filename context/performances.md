# Performances de la Context API

## Simple context 
> Dans cet exemple, nous utilisons `props.children`pour faire passer les composants au context provider.

```js
import React, { useContext, useState, createContext } from "react";
import ReactDOM from "react-dom";

const AppContext = createContext();

const AppProvider = (props) =>
{
  const [count, setCount] = useState( 0 );
  const value = { count, setCount };
  return (
    <AppContext.Provider value={ value }>
      {props.children}
    </AppContext.Provider>
  )
}

const CountDisplay => ()
{
  const { count } = useContext( AppContext );
  return <h2>The Count is: {count}</h2>
}

const CountButton = () =>
{
  const { setCount } = useContext( AppContext );
  return (
    <button
    	onClick={ () => setCount(count => count + 1) }
    >
      Increment
    </button>
  )
}

/* Just for fun, make a doom hierarchy */
const OuterWrapper = () => <InnerWrapper />
const InnerWrapper = () => <CountButton />

const App = () =>
{
  return (
    <div>
      <AppProvider>
        <CountDisplay/>
        <OuterWrapper/>
      </AppProvider>
    </div>
  )
}

reactDOM.render( <App />, document.getElementById('⚛️') )
```

## Custom context hook
> Kent C. Dodds indique qu'il est intéressant d'encapsuler la *context* dans un *hook*

```js
import React, { useContext, useState, createContext } from "react";
import ReactDOM from "react-dom";

const AppContext = createContext();

/* Create a custom hook to wrap the context */
const useAppContext = () =>
{
  const context = useContext( AppContext );
  if( !context )
  {
    throw new Error( 'AppContext missing !' );
  }
  return context;
}

const AppProvider = (props) =>
{
  const [ count, setCount ] = useState( 0 )
  // Little trick : memoization for internal values !
  const value = useMemo( () => ({ count, setCount }), [count] )
  return <AppContext.Provider value={ value } {...props} />
}

const CountDisplay => ()
{
  const { count } = useAppContext()//<--- Custom hook with destructuration
  return <h2>The Count is: {count}</h2>
}

const CountButton = () =>
{
  const { setCount } = useAppContext()//<--- Custom hook with destructuration
  return (
    <button
    	onClick={ () => setCount(count => count + 1) }
    >
      Increment
    </button>
  )
}

/* Just for fun, make a doom hierarchy */
const OuterWrapper = () => <InnerWrapper />
const InnerWrapper = () => <CountButton />

const App = () =>
{
  return (
    <div>
      <AppProvider>
        <CountDisplay/>
        <OuterWrapper/>
      </AppProvider>
    </div>
  )
}

reactDOM.render( <App />, document.getElementById('⚛️') )
```

A retenir :

1. Lancement d'une exception au cass où le *context* ne serait pas défini.
1. Memoïsation de toutes les valeurs du *context* au sein du composant *provider*.
1. Diffusion des `props` plutôt qu'utilisation de `props.children`, technique très élégante.

## Scalability des *context*
**Attention !** Tout composant dont les états reposent sur un *context* sera **raffraichi à chaque modification d'une de ses valeur !!**

**Pire encore**, si vous utilisez un context global, ce sont **toutes les hiérarches de vos composants qui sont systématiquement raffraichies**, lors du changement d'une des valeurs.

```js
import React, { useContext, useState, createContext } from "react";
import ReactDOM from "react-dom";

const AppContext = createContext();

const useAppContext = () =>
{
  const context = useContext( AppContext );
  if( !context )
  {
    throw new Error( 'AppContext missing !' );
  }
  return context;
}

const AppProvider = (props) =>
{
  const [ count, setCount ] = useState( 0 );
  // this message never changes !
  const [message, setMessage] = useState('Hello from Context!');
  const value = 
  {
    count,
    setCount,
    message,
    setMessage
  };
  return <AppContext.Provider value={ value } {...props}/>
}

const Message = () => 
{
  const { message } = useAppContext();
  // the text will render to a random color for
  // each instance of the Message component
  const getColor = () => ( Math.floor( Math.random() * 255 ) );
  const style = { color: `rgb(${ getColor() },${ getColor() },${ getColor() })` }
  return (
    <div>
      <h4 style={ style }>{ message }</h4>
    </div>
  )
}

const Count = () =>
{
  const { count, setCount } = useAppContext()
  return (
  <div>
    <h3>Current count from context: { count }</h3>
    <button
    	onClick={ () => setCount(count => count + 1) }
    >
      Increment
    </button>
  </div>
  )
}

const App = () =>
{
  return (
    <div>
      <AppProvider>
        <h2>Re-renders! 😩</h2>
        <Message />
        <Message />
        <Message />
        <Count />
      </AppProvider>
    </div>
  )
}
reactDOM.render( <App />, document.getElementById('⚛️') );
```

[See it live here](https://codepen.io/heticschool/pen/xxGmWZK)

Alors faut-il éliminer son usage dans de gosses applications ?
1. Tant que vous ne constatez pas de problème de performances
1. Utilisez un framework tel que [Redux](https://redux.js.org/), [Mobx](https://mobx.js.org/) ou [Undux](https://undux.org/)
1. [Repensez l'ensemble de la struture des états, et divisez-les](context_wise.md). 

## Références
- https://leewarrick.com/blog/the-problem-with-context/
- https://frontarm.com/james-k-nelson/when-context-replaces-redux/
- https://frontarm.com/james-k-nelson/react-context-performance/