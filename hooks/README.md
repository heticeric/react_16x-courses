# React hooks

## Notion d'état dans les composants
Auparavant, seuls les composants crées à partir d'une classe pouvait gérer un état applicatif.
La propriété `this.state` et sa méthode `this.setState()` servent à manipuler les valeurs de cet état.

Depuis la version 16.8 de `React`, les composants qui reposent sur une fonction, peuvent également manipuler un état interne. Suivez le guide !

## La fonction `useState`

Partons du composant suivant :

```js
import React from 'react';
const students = 
[
  {
    id: '123',
    name: 'Priou',
  },
  {
    id: '456',
    name: 'Masselot',
  },
  {
    id: '789',
    name: 'Bruant',
  },
];
export default () => 
(
  <div>
    <ul>
    {
        students.map
        (
            s => 
            (
                <li key={ s.id }>
                    <label>{ s.name }</label>
                </li>
            )
        )
    }
    </ul>
  </div>
);
```

Si nous souhaitons lui ajouter un état interne, nous pouvons l'initialiser avec des valeurs par défaut :

```js
import React from 'react';
const students = 
[
  {
    id: '123',
    name: 'Priou',
  },
  {
    id: '456',
    name: 'Masselot',
  },
  {
    id: '789',
    name: 'Bruant',
  },
];

export default () => 
{
    // 1- La première valeur correspond à la vvariable de l'état
    // 2- La seconde valeur est une fonction qui permet de modifier cette valeur
    const [ all_students, setStudent ] = React.useState( students );

    return (
        <div>
            <ul>
            {
                all_students.map
                (
                    s => 
                    (
                        <li key={ s.id }>
                            <label>{ s.name }</label>
                        </li>
                    )
                )
            }
            </ul>
        </div>
    )
};
```

Puis, nous pouvons muter la valeur ainsi :

```js
import React from 'react';
const students = 
[
  {
    id: '123',
    name: 'Priou',
  },
  {
    id: '456',
    name: 'Masselot',
  },
  {
    id: '789',
    name: 'Bruant',
  },
];

export default () => 
{
    const [ all_students, addStudent ] = React.useState( students );
    // Nouvel état pour le composant `input`
    const [ student_name, setStudentName ] = React.useState( students );

    return (
        <div>
            <ul>
            {
                all_students.map
                (
                    s => 
                    (
                        <li key={ s.id }>
                            <label>{ s.name }</label>
                        </li>
                    )
                )
            }
            </ul>
            <form>
                <input
                    type="text"
                    value={ student_name }
                    onChange={ e => setStudentName( e.target.value ) }
                >
                <button type="submit">Add student</button>
            </form>
        </div>
    )
};
```

Vous remarquerez l'absence de `this` puisque nous sommes dans la portée (closure) du composant.

De même, pour ajouter une valeur depuis le formulaire

```js
import React from 'react';
const students = 
[
  {
    id: '123',
    name: 'Priou',
  },
  {
    id: '456',
    name: 'Masselot',
  },
  {
    id: '789',
    name: 'Bruant',
  },
];

export default () => 
{
    const [ all_students, setStudent ] = React.useState( students );
    // Nouvel état pour le composant `input`
    const [ student_name, setStudentName ] = React.useState( students );

    return (
        <div>
            <ul>
            {
                all_students.map
                (
                    s => 
                    (
                        <li key={ s.id }>
                            <label>{ s.name }</label>
                        </li>
                    )
                )
            }
            </ul>
            <form
                onSubmit=
                { 
                    e => 
                    {
                        e.preventDefault(); // Attention à ne pas raffraîchir la page
                        setStudent
                        (
                            all_student.push( { id:Math.random()*999, name : student_name } )
                        )
                    }
                }
            >
                <input
                    type="text"
                    value={ student_name }
                    onChange={ e => setStudentName( e.target.value ) }
                >
                <button type="submit">Add student</button>
            </form>
        </div>
    )
};           
```

## La fonction `useReducer`

Cette fonction permet de gérer des cas plus complexes de manipulation des états.
En effet, au lieu de modifier directement la valeur, `useReducer` accepte deux arguments : l'état courant et une action afin de modifier l'état :

```
(state, action) => newState
```

Concrètement, la fonction reçoit un appel, qui contiendra une chaîne de caractère (l'action) et qui permettra de modifier l'état courant via une nouvelle valeur (payload)

```js
export default () => 
{
  const initialTodos = 
  [
    {
        id: 'a',
        task: 'Learn React',
        complete: false,
    },
    {
        id: 'b',
        task: 'Learn Firebase',
        complete: false,
    },
  ];

  const todoReducer = (state, action) => 
  {
    switch (action.type)
    {
        case 'DO_TODO':
            return state.map
            (
                todo =>
                {
                    if (todo.id === action.id) return { ...todo, complete: true };
                    else return todo;
                }
            );
        case 'UNDO_TODO':
            return state.map
            (
                todo => 
                {
                    if (todo.id === action.id) return { ...todo, complete: false };
                    else return todo;
                }
            );
        default:
            return state;
    }
  };

  const [todos, dispatch] = React.useReducer
  (
    todoReducer,
    initialTodos
  );

  const handleChange = todo => 
  {
    dispatch
    (
        {
            type: todo.complete ? 'UNDO_TODO' : 'DO_TODO',
            id: todo.id,
        }
    );
  };

  return
  (
    <ul>
    {
        todos.map
        (
            todo => 
            (
                <li key={todo.id}>
                <label>
                    <input
                        type="checkbox"
                        checked={todo.complete}
                        onChange={() => handleChange(todo)}
                    />
                    {todo.task}
                </label>
                </li>
            )
        )
    }
    </ul>
  );
};
```

Cette méthodologie n'est pas sans rappeler le *framework* `Redux`. Bien au contraire !

## Obtenir les données d'un contexte `Provider` parent

`Hook` par ci, `hook` par là.

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

## Hook hook hook

- [Brightleaf React Hooks](https://brightleaf.dev/hooks/)
- [Awesome React Hooks Resources](https://github.com/rehooks/awesome-react-hooks)
- [Collection of React Hooks](https://nikgraf.github.io/react-hooks/)
