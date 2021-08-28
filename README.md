# React Hooks

### `useState`
❕ The useState Hook lets us keep local state in a function component.

<br/>
<details><summary> ✏︎ <b>Code Example</b></summary>

```javascript
import React, { useState } from 'react';

const Clicker = () => {
  const [clicks, setClicks] = useState(0);
  const [showClicks, setShowClicks] = useState(false);

  const onClick = () => {
    setClicks(clicks + 1);
  };

  const onToggleShowClicks = () => {
    setShowClicks((prevValue) => !prevValue);
  };

  const clicksText = showClicks ? ` [${clicks}]` : '';

  return (
    <div>
      <button onClick={onClick}>Click me! {clicksText}</button>
      <button onClick={onToggleShowClicks}>Toggle show clicks</button>
    </div>
  );
};

export default Clicker;
```
```javascript
```
</details>
<br/><br/>

### `useEffect`
❕ The Effect Hook lets you perform side effects in function components.

> If you’re familiar with React class lifecycle methods, you can think of useEffect Hook as componentDidMount, componentDidUpdate, and componentWillUnmount combined.
<br/>
<details><summary> ✏︎ <b>Code Example</b></summary>

```javascript
import React, { useState, useEffect } from 'react';

const App = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log(`>> running effect ${count}`);
    return () => {
      console.log(`<< cleaning up ${count}`);
    };
  }, [count]);

  useEffect(() => {
    console.log('component did mount');
    return () => {
      console.log('component will unmount');
    };
  }, []);

  useEffect(() => {
    console.log('executed AFTER each render');
  }); // deps list is missing

  const increment = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <h3>{count}</h3>
      <button onClick={increment}>+</button>
    </div>
  );
};

export default App;
```
</details>
<br/><br/>
