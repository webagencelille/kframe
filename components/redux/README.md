## aframe-redux-component

> Depends on upcoming A-Frame 0.3.0.

A [Redux](http://redux.js.org/) component for [A-Frame](https://aframe.io).

This component provides simple hooks into A-Frame for creating reducers,
setting up component data-bindings to the state, and dispatching actions. All
without React. What's cooler than Redux VR?

### Usage

The Redux system will create the store using passed in reducers:

```html
<a-scene redux="reducers: player, enemy"></a-scene>
```

To register reducers, we can use the `AFRAME.registerReducer` API:

```js
AFRAME.registerReducer('player', {
  actions: {
    DAMAGE: 'PLAYER__DAMAGE'
  },

  initialState: {
    health: 100
  },

  reducer: function (state, action) {
    state = state || this.initialState;
    switch (action.type) {
      case this.actions.DAMAGE: {
        var newState = Object.assign({}, state);
        newState.health -= action.payload;
        return newState;
      }
    }
  }
});
```

The `redux` component handles data binding. The component subscribes to the
store to update an entity's components on every change. The keys of the
component are state selectors, using dot-delimitation to reach deeper into the
state. The values of the component define what components to data-bind the
state property to.

This will bind `reduxState.player.health` to the `opacity` component.

```html
<a-entity redux="player.health: opacity"></a-entity>
```

To dispatch an action, we can use the Redux system's `dispatch` method.

```js
document.querySelector('a-scene').systems.redux.dispatch(AFRAME.reducers.player.DAMAGE, 50);
```

### API

#### `redux` System

| Property | Description                                  |
| -------- | -----------                                  |
| reducers | Comma-separated list of registered reducers. |

#### `redux` Component

| Property Description          | Value Description                 |
| --------                      | -----------                       |
| Dot-separated state selector. | Target component to data-bind to. |

### Installation

#### Browser

Install and use by directly including the [browser files](dist):

```html
<head>
  <title>My A-Frame Scene</title>
  <script src="https://aframe.io/releases/0.3.0/aframe.min.js"></script>
  <script src="https://rawgit.com/ngokevin/aframe-redux-component/master/dist/aframe-redux-component.min.js"></script>
</head>

<body>
  <a-scene redux="reducers: player">
    <a-entity redux="player.health: opacity"></a-entity>
  </a-scene>
</body>
```

#### npm

Install via npm:

```bash
npm install aframe-redux-component
```

Then register and use.

```js
require('aframe');
require('aframe-redux-component');
```