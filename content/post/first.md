+++
date = "2016-01-12T13:59:48-05:00"
title = "first"
image = "images/cover18.jpeg"
+++


Intro text goes here...

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

#### redux

Something about Redux, link to egghead, twitter etc

#### rxjs

Excerpt about rxjs from docs, staltz on 'scan' method
rxjs in angular, linke to thoughtram

#### illustration

diagram

We'll need two streams:

```javascript
var state$ = new Rx.Subject(),
  actions$ = new Rx.Subject();

var initialState = {
  count: 0
};

var reducer = function(state, action) {
  switch (action && action.key) {
    case "INCREMENT":
      var new_state = state;
      new_state.count += 1;
      return new_state;
    default:
      return state;
  }
}

var combined = actions$.scan(reducer, initialState).subscribe((state) => {
  state$.next(state);
})

state$.subscribe(
  ({count}) => {
      ReactDOM.render( <p> Count: {count} </p>, document.getElementById('container'));
})

setInterval(function() {
  actions$.next({key: "INCREMENT"});
}, 1500)
```

<script async src="http://jsfiddle.net/naderhen/w7fucdhz/8/embed/"></script>


What if we want two? combinereducers

```javascript
var combineReducers = function combineReducers(reducers) {
  return function (state, action) {
    if (state === undefined) state = {};

    return Object.keys(reducers).reduce(function (nextState, key) {
      nextState[key] = reducers[key](state[key], action);
      return nextState;
    }, {});
  };
};
```
