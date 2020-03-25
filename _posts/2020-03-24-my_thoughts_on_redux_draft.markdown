---
layout: post
title:      "My Thoughts on Redux "
date:       2020-03-24 22:48:59 -0400
permalink:  my_thoughts_on_redux_draft
---


After completing my React project, I can finally see the benefits of using a predictable state container like [Redux](https://redux.js.org/). Once I got over the fact that it has a lot of boilerplate and set up, I became a big fan of it. Being able to write an application that behaves consistently and able to debug efficiently with built in tools makes Redux incredibly powerful. The entire library including dependencies only take 2kb of space and not to mention, it has plenty of addon packages available like the [Redux Toolkit](https://redux-toolkit.js.org/). Installing Redux in your React app is not that bad at all, the [official docs](https://redux.js.org/introduction/installation) recommend using the [official Redux+JS template](https://github.com/reduxjs/cra-template-redux) but it is possible to create the React app normally and later incorporate Redux into the app once it is truly needed. 

```javascript
import React from "react";
import ReactDOM from "react-dom";
import "./styles/index.css";
import App from "./containers/App";

import { Provider } from "react-redux";
import { createStore, applyMiddleware, compose } from "redux";
import thunk from "redux-thunk";
import rootReducer from "./reducers/index.js";
// import root reducer

// Allows using Redux Dev Tools
const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
const store = createStore(
  rootReducer,
  composeEnhancers(applyMiddleware(thunk))
);

// For using Redux without the Dev Tools
// const store = createStore(
//   rootReducer,
//   applyMiddleware(thunk)
// );

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);

```

The snippet above goes inside the index.js file of the React app and it gives access to the Redux store. ![](https://media.giphy.com/media/mVk3rRuD8KUko/giphy.gif)

to be continued
