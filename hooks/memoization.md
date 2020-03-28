### `useMemo()`
`useMemo` est utilisé pour [*mémoïser*](https://fr.wikipedia.org/wiki/M%C3%A9mo%C3%AFsation) la valeur de retournée par une fonction. Ainsi, celle-ci ne sera ré-utilisée que lorsque l'une de ses dépendances change.

> Exemple sans mémoïsation

```js
import React, { useState } from "react";

export default () =>
{
  const [msg, setMsg] = useState("hello");
  const reverseMsg = () => msg.split("").reverse().join("");
  
  return (
    <div>
      <h1>{ msg }</h1>
      <h1>{ reverseMsg() }</h1>
      <h1>{reverseMsg()}</h1>
      <button onClick={() => setMsg( "Hello" )}>Change Msg</button>
    </div>
  );
}
```
Dans cet exemple, la fonction `reverseMsg` est utilisée deux fois, alors qu'elle retourne le même résultat.
Voilà une bonne occasion d'utiliser une version *mémoïsée* de sa valeur.

> Exemple avec mémoïsation

```js
import React, { useState} from "react";

export default() =>
{
  const [msg, setMsg] = useState("hello");
  const reverseMsg = useMemo
  (
  	() => msg.split("").reverse().join("")
  	,[ msg /* msg is the dependency */ ]
  );
  
  return (
    <div>
      <h1>{ msg }</h1>
      <h1>{ reverseMsg }</h1>
      <h1>{ reverseMsg }</h1>
      <button onClick={() => setMsg("Hello")}>Change Msg</button>
    </div>
  );
}
```
`useMemo` reçoit comme 2nd paramètre un tableau.
**`useMemo` ne relancera la fonction que si sa dépendance a été modifiée.**

Pensez à ajouter les éventuels arguments dans le tableau de dépendances :
```js
const memoizedValue = useMemo
(
	() => expensiveOperation( param1, param2 )
	,[ param1, param2]
)
```

### `useCallback()`

Ce *hook* est similaire à `useMemo`, la différence est que `useCallback` retourne une version memoïsée de la référence d'un *callback* et que `useMemo` retourne une valeur memoïsée, résultant de cette fonction.
**Cela permet notamment d'éviter qu'une fonction soit redéfinie lors de chaque raffraichissement.**

De plus, `useCallback` est souvent utilisé pour des *callbacks* passés des des composants enfants, afin que ceux obtiennent une référence mise en cache.

> Exemple sans mémoïsation

```js
import React, { useState } from "react";
import ReactDOM from "react-dom";

const Button = (props) => <button onClick={ props.onClick }>{ props.name }</button>

const App = () =>
{
  const [ count, setCount ] = useState( 0 );
  const [ isActive, setActive ] = useState( false );

  const handleCount = () => setCount( count + 1 );
  const handleShow = () => setActive( ! isActive );
  
  return (
    <div className="App">
	    {
	    	isActive 
	    	&&
	    	(	<div>
	    			<h1>{count}</h1>
					<Button onClick={ handleCount } name="Increment" />
				</div>
			)
	    }
	    <Button
	    	onClick={ handleShow }
	       name={ isActive ? "Hide Counter" : "Show Counter" }
	    />
    </div>
  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```

Lorsque que l'on clique sur l'un des boutons, leur interface est raffraichie, et leurs fonctions sont recrées.

> Exemple avec mémoïsation
 
```js
import React, { useState, useCallback } from "react";
import ReactDOM from "react-dom";

const Button = (props) => <button onClick={ props.onClick }>{ props.name }</button>

const App = () =>
{
  const [ count, setCount ] = useState( 0 );
  const [ isActive, setActive ] = useState( false );

  const handleCount = useCallback( () => setCount( count + 1 ), [count]);
  const handleShow = useCallback( () => setActive( ! isActive ), [isActive]);
  
  return (
    <div className="App">
	    {
	    	isActive 
	    	&&
	    	(	<div>
	    			<h1>{count}</h1>
					<Button onClick={ handleCount } name="Increment" />
				</div>
			)
	    }
	    <Button
	    	onClick={ handleShow }
	       name={ isActive ? "Hide Counter" : "Show Counter" }
	    />
    </div>

  );
}

ReactDOM.render(<App />, document.getElementById("root"));
```

De la même façon que `useMemo`, le 2nd paramètre définit un tableau de dépendances.
Lorsque l'une change, la fonction est raffraichie dans le cache de la mémoïsation.

Dans l'exemple précédent, si nous clliquons sur le bouton *Increment*, seule la fonction `handleCount` est relancée.


### Dans quels cas utiliser ces hooks ?
**Le conseil est d'éviter l'usage de fonctionnalités, tant que des problèmes de performances ne surgissent pas.**

#### Références
- https://kentcdodds.com/blog/usememo-and-usecallback