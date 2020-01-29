---
title: Resize event listener using React hooks
date: "2020-01-09"
template: "post"
draft: false
slug: "resize-event-listener-using-react-hooks"
category: "Tutorial"
tags:
  - "react"
  - "npm"
  - "javascript"
  - "tutorial"
  - "react-hooks"
description: "A couple of weeks ago while I was working on a small React project I had to implement some custom logic for the scenario when the user resizes the browser window."
socialImage: "/media/programmer.png"
---

![Resize event listener using React hooks](/media/programmer.png)

A couple of weeks ago while I was working on a small **React** project I had to implement some custom logic for the scenario when the user resizes the browser window.

The usual **Javascript** solution looks like this.
```js
window.addEventListener('resize', function() {
  // your custom logic
});
```

This one can be used successfully, but it is not looking very good in a **React** app. So I decided to implement it differently using a more familiar approach for **React** developers, called **hooks**. Hooks are functions that let you “hook into” **React** state and lifecycle features from function components.

A **hook** that will return the window width and update it when it changes can look like this.

```js
import { useState, useEffect } from 'react';

const getWidth = () => window.innerWidth
  || document.documentElement.clientWidth
  || document.body.clientWidth;

function useCurrentWitdh() {
  // save current window width in the state object
  let [width, setWidth] = useState(getWidth());

  // in this case useEffect will execute only once because
  // it does not have any dependencies.
  useEffect(() => {
    const resizeListener = () => {
      // change width from the state object
      setWidth(getWidth())
    };
    // set resize listener
    window.addEventListener('resize', resizeListener);

    // clean up function
    return () => {
      // remove resize listener
      window.removeEventListener('resize', resizeListener);
    }
  }, [])

  return width;
}
```

I used **useState** hook to keep window width value in the state object and **useEffect** to add a listener for the **resize** event.

There is one more thing to mention, **resize** event it is triggered multiple times while the user is actively dragging the browser's window resize handle, it may affect the performance of your application if you have a complex logic in the **resize** listener. One way to deal with it is to implement some debounce mechanism that will limit the rate at which a function can fire.

```js
import { useState, useEffect } from 'react';

const getWidth = () => window.innerWidth
  || document.documentElement.clientWidth
  || document.body.clientWidth;

function useCurrentWitdh() {
  // save current window width in the state object
  let [width, setWidth] = useState(getWidth());

  // in this case useEffect will execute only once because
  // it does not have any dependencies.
  useEffect(() => {
    // timeoutId for debounce mechanism
    let timeoutId = null;
    const resizeListener = () => {
      // prevent execution of previous setTimeout
      clearTimeout(timeoutId);
      // change width from the state object after 150 milliseconds
      timeoutId = setTimeout(() => setWidth(getWidth()), 150);
    };
    // set resize listener
    window.addEventListener('resize', resizeListener);

    // clean up function
    return () => {
      // remove resize listener
      window.removeEventListener('resize', resizeListener);
    }
  }, [])

  return width;
}
```

How this debounce mechanism works? At every **resize** event fired, we delay the changing of the state object with 150 milliseconds, in case that another **resize** event gets fired before, it will prevent the previous change of the state to happen. In other words, the change of the state can happen only once every 150 milliseconds. More about this technique you can read on [David Walsh blog](https://davidwalsh.name/javascript-debounce-function).

Example of how to use this hook in a **React** component.

```js
const App = () => {
  let width = useCurrentWitdh();

  return (
    <div>
      <h1>
        {`Current width -> ${width}`}
      </h1>
    </div>
  );
}
```

Complete source code can be found [here](https://github.com/vitaliemaldur/react-breakpoints-hook). Feedback is much appreciated.
