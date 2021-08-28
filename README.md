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
### Form
```javascript
import React, { useState } from 'react';

const Field = (props) => {
  const { name, label, value, type = 'text', onChange } = props;
  return (
    <div>
      <label htmlFor={name}>{label}</label>
      <input
        name={name}
        type={type}
        value={value}
        onChange={(event) => onChange(event.target.value)}
      />
    </div>
  );
};

const Form = () => {
  const [firstName, setFirstName] = useState('');
  const [lastName, setLastName] = useState('');
  const [age, setAge] = useState(0);

  const clearForm = () => {
    setFirstName('');
    setLastName('');
    setAge(0);
  };

  return (
    <div>
      <form>
        <Field
          name="firstName"
          label="First Name"
          value={firstName}
          onChange={(newValue) => setFirstName(newValue)}
        />
        <Field
          name="lastName"
          label="Last Name"
          value={lastName}
          onChange={(newValue) => setLastName(newValue)}
        />
        <Field
          name="age"
          label="Age"
          type="number"
          value={age}
          onChange={(newValue) => setAge(newValue ? parseInt(newValue) : 0)}
        />
      </form>
      <button onClick={clearForm}>Clear</button>
      <div>First Name: {firstName}</div>
      <div>Last Name: {lastName}</div>
      <div>Age: {age}</div>
    </div>
  );
};

export default Form;
```
</details>
<br/>

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
<br/>
