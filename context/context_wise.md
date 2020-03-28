# Scalable react app

## Bonnes pratiques
### Encapsuler la logique de manipulation des états dans les context provider

```js
const useCount = () =>
{
  const context = React.useContext( AppContext );
  if (!context) {
    throw new Error(`AppContext missing !`)

  }

  const [ count, setCount ] = context;

  /* Encapsulate state logic */
  const increment = () => setCount( c => c + 1 );
  return {
    count,
    setCount,
    increment,
  }
}

const CountButton = () =>
{
  const { increment } = useContext( AppContext );
  return (
    <button
    	onClick={ increment }
    >
      Increment
    </button>
  )
}
```

### Utiliser `useReducer` lorsque cela se complexifie

```js
const countReducer = (state, action) =>
{
  switch( action.type )
  {
    case 'INCREMENT': return {count: state.count + 1};
    default: throw new Error(`Unsupported action type: ${ action.type }`);
  }

}

const AppProvider = (props) =>
{
	const [ state, dispatch ] = React.useReducer( countReducer, { count: 0 } );
	const value = React.useMemo(() => [state, dispatch], [state]);
	return <AppContext.Provider value={ value } {...props} />
}


const useCount = () =>
{
  const context = React.useContext( AppContext );
  if (!context) {
    throw new Error(`AppContext missing !`)

  }

  /* ala Redux */
  const [state, dispatch] = context;
  const increment = () => dispatch( {type: 'INCREMENT'} );

  return {
    count,
    setCount,
    increment,
  }
}

const CountButton = () =>
{
  const { increment } = useContext( AppContext );
  return (
    <button
    	onClick={ increment }
    >
      Increment
    </button>
  )
}
```

#### Références
- https://kentcdodds.com/blog/how-to-use-react-context-effectively

### Multi contexts to the rescue
En suivant le principe de la [colocation](optim/README.md) :
- Toutes vos valeurs ne doivent pas nécessairement être déclarées dans un seul objet. Conservez les éléments séparés et locaux (les paramètres utilisateur ne doivent pas nécessairement être définis dans le même context que les notifications !). **Définissez de multiple context providers**.
- Vos context n'ont pas tous besoin d'être accessibles globalement. Conservez vos context aussi prôches que possible de leur usage.

> Travail pratique : Implémenter cette application

```js
const App = () =>
{
  return (
    <ThemeProvider>
      <AuthenticationProvider>
        <Router>
          <Home path="/" />
          <About path="/about" />
          <UserPage path="/:userId" />
          <UserSettings path="/settings" />
          <Notifications path="/notifications" />
        </Router>
      </AuthenticationProvider>
    </ThemeProvider>
  );
}

const Notifications = () =>
{
  return (
    <NotificationsProvider>
      <NotificationsTab />
      <NotificationsTypeList />
      <NotificationsList />
    </NotificationsProvider>
  );
}

const UserPage = ({username}) =>
{
  return (
    <UserProvider username={username}>
      <UserInfo />
      <UserNav />
      <UserActivity />
    </UserProvider>
  );
}

const UserSettings = () =>
{
  // Associated hook for the AuthenticationProvider
  const { user } = useAuthenticatedUser();
}
```

#### Références
- https://kentcdodds.com/blog/application-state-management-with-react

