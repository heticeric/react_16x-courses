# Colocation des états

## De l'importance de l'emplacement d'un états applicatif
La colocation s'attache à laisser la défintion des états le plus prôche possible de leur emploi.

> Exemple sans colocation 

```js
import React from 'react'
import ReactDOM from 'react-dom'

function sleep(time) {
  const done = Date.now() + time
  while (done > Date.now()) {
    // sleep...
  }
}

// imagine that this slow component is actually slow because it's rendering a
// lot of data (for example).
function SlowComponent({time, onChange}) {
  sleep(time)
  return (
    <div>
      Wow, that was{' '}
      <input
        value={time}
        type="number"
        onChange={e => onChange(Number(e.target.value))}
      />
      ms slow
    </div>
  )
}

function DogName({time, dog, onChange}) {
  return (
    <div>
      <label htmlFor="dog">Dog Name</label>
      <br />
      <input id="dog" value={dog} onChange={e => onChange(e.target.value)} />
      <p>{dog ? `${dog}'s favorite number is ${time}.` : 'enter a dog name'}</p>
    </div>
  )
}

function App() {
  // this is "global state"
  const [dog, setDog] = React.useState('')
  const [time, setTime] = React.useState(200)
  return (
    <div>
      <DogName time={time} dog={dog} onChange={setDog} />
      <SlowComponent time={time} onChange={setTime} />
    </div>
  )
}

reactDOM.render(<App />, document.getElementById('⚛️'))
```

> Le même exemple avec colocation des états

```js
import React from 'react'
import ReactDOM from 'react-dom'

function sleep(time) {
  const done = Date.now() + time
  while (done > Date.now()) {
    // sleep...
  }
}

// imagine that this slow component is actually slow because it's rendering a
// lot of data (for example).
function SlowComponent({time, onChange}) {
  sleep(time)
  return (
    <div>
      Wow, that was{' '}
      <input
        value={time}
        type="number"
        onChange={e => onChange(Number(e.target.value))}
      />
      ms slow
    </div>
  )
}

function DogName({time}) {
  const [dog, setDog] = React.useState('') // <--- colocated state
  return (
    <div>
      <label htmlFor="dog">Dog Name</label>
      <br />
      <input id="dog" value={dog} onChange={e => setDog(e.target.value)} />
      <p>{dog ? `${dog}'s favorite number is ${time}.` : 'enter a dog name'}</p>
    </div>
  )
}

function App() {
  // this is "global state"
  const [time, setTime] = React.useState(200)
  return (
    <div>
      <DogName time={time} />
      <SlowComponent time={time} onChange={setTime} />
    </div>
  )
}

reactDOM.render(<App />, document.getElementById('⚛️'))
```

Vous comprenez à quel point il est crucial de savoir définir la définition de vos états.
C'est d'autant plus vrai lorsque sont utilisés des frameworks tels que `Redux`, qui tendent à **définir tous les états d'une application dans un store global**.

![Decision tree on state defintion](media/where-to-put-state.png)

## Références
- https://kentcdodds.com/blog/state-colocation-will-make-your-react-app-faster