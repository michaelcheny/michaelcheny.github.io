---
layout: post
title:      "React Hooks"
date:       2020-01-25 07:09:48 +0000
permalink:  react_hooks
---


We are two weeks into React now, and I can honestly say that I am enjoying this much more than vanilla Javascript. Vanilla JavaScript isn't bad or negative, but React is simply providing a much greater user experience. [React](https://reactjs.org) is a JavaScript library created and maintained by Facebook for building user interfaces. It breaks down the process of building out an entire app by simplifying it into seperate components, which together, makes complex user interfaces. The idea of building out modular components, and having the ability to reuse those components in future projects makes React very powerful. Also, keeping the components seperate allows us to debug and change something much easier without destroying the entire application and causing panics. This is only a small preview of React's powers, other notable features include the Virtual DOM, event handling, performance, and not to mention, React Native (for building mobile apps).

One of my favorite things about React is that it is actively maintained by Facebook, and improving on a day to day basis. The main feature I want to talk about is about React Hooks. I don't really see it being covered in the curriculum, but that is because it is still very new, being released with [React 16.8.0](https://github.com/facebook/react/blob/master/CHANGELOG.md#1680-february-6-2019). Hooks in React allows us to use state and other features without writing a class. This is very powerful because prior to hooks, it was tougher to reuse stateful logic between components, meaning it was difficult to "attach" reusable behavior to a component (the official doc gives an example of connecting it to a store). With the introduction of Hooks, it is now easier to reuse stateful logic without changing the component hierarchy. I recommend watching the React Conf 2018 video since the people in the React team covers everything about Hooks in detail. 

[![React Conf 2018](http://img.youtube.com/vi/dpw9EHDh2bM/0.jpg)](http://www.youtube.com/watch?v=dpw9EHDh2bM "React Conf 2018") 

After finishing the "Async React Mini Project" lab in the Async React section, I decided to redo the entire lab with Hooks incorporated. In one of the components, I was able to refactor the classical component into a functional component while making the code look and feel much cleaner. I started out with this:

```
import React, { Component } from "react";
import GifList from "../components/GifList";
import GifSearch from "../components/GifSearch";

export class GifListContainer extends Component {
  constructor(props) {
    super(props);
    this.state = {
      gifs: []
    };
  }

  componentDidMount() {
    this.submitSearch();
  }

  submitSearch = async inputQuery => {
    const res = await fetch(
      `https://api.giphy.com/v1/gifs/search?q=${inputQuery}&api_key=dc6zaTOxFJmzC&rating=g`
    );
    const { data } = await res.json();
    this.setState({
      gifs: data
    });
  };

  render() {
    return (
      <div>
        <GifSearch submitCallback={this.submitSearch} />
        <GifList gifs={this.state.gifs} />
      </div>
    );
  }
}
```

And I was able to shrink it down into this:

```
import React, { useState, useEffect } from "react";
import GifList from "../components/GifList";
import GifSearch from "../components/GifSearch";
const GifListContainer = () => {
  const [gifs, setGifs] = useState([]);

  useEffect(() => {
    submitSearch();
  }, []);

  const submitSearch = async (inputQuery = "water") => {
    const res = await fetch(
      `https://api.giphy.com/v1/gifs/search?q=${inputQuery}&api_key=dc6zaTOxFJmzC&rating=g`
    );
    const { data } = await res.json();
    setGifs(data);
  };

  return (
    <div>
      <GifSearch submitCallback={submitSearch} />
      <GifList gifs={gifs} />
    </div>
  );
};
```

This resulted in a component that became much easier to work with, especially if I decided to add more in more complex logic. `const [gifs, setGifs] = useState([]);` Over here, we declare a new state variable, called `gifs` and set it to an empty array. The second variable `setGifs` can be named whatever we want, but it if convention to name it the same as our first one, with a prefix of `set`, and this variable replaces `this.setState` from our classical component. With the `setState()` Hook, it is a lot easier to update the state of `gifs`, we just simply pass in the new value of `gifs` into `setGifs("just shove it in here")`.

Example:

```
<button onClick={() => setLikes(like + 1)}>
  Like this post!
</button>
```

If you thought that was cool, check out the `useEffect()` Hook.

```
  useEffect(() => {
    submitSearch();
  }, []);
```

The `useEffect()` Hook pretty much covers all of the lifecycle methods a component may have. In the code snippet above, I passed in an empty array to the optional second argument of `useEffect()` to replace `componentDidMount()`, they do the same thing. If we were working with subscriptions (`setInterval()`) and wanted to clear it during the `componentWillUnmount()` lifecycle method, we would simply throw in a return at the end of `useEffect()` like this:

```
useEffect(() => {
    // ...
    const example = setInterval(function, milliseconds);
    return () => {
      clearInterval(example);
    };
});
```

We can also optimize the performance of the application by skipping effects, or updating them only when certain values change. If we wanted a specific part of the DOM to update only when a specific state or prop changes, we could pass that variable into the optional array like so:

```
useEffect(() => {
  document.title = `${count} likes!`
  console.log(`You smashed the like button ${count} times!`)
}, [count]); // Only re-renders if count changes, otherwise, do nothing.
```

This is only a small preview to show the powers of Hooks in React. While this is still fairly new, I think it's a good idea to get exposed to the concept of Hooks. I find Hooks much more user friendly compared to classes, since it does almost everything classes can, without sacrificing much (just make sure to follow the [rules](https://reactjs.org/docs/hooks-rules.html)). The official React Doc states that "There are no plans to remove classes from React", so you don't have to refactor every single one of your classes into functions using Hooks, but instead, you can start to incorporate Hooks in the new code side by side with classes. It will be interesting to see what new and amazing features the React team will deliver in the future. Hopefully they will be game-changers like Hooks.

