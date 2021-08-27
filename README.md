# React Hooks
### `useEffect()`
<br/>
<details><summary> 📝 <b>Code Example</b></summary>

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
