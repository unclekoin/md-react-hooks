# React Hooks

### `useState`
❕ The useState Hook lets us keep local state in a function component.

<br/>
<details><summary><b>Code Example</b></summary>

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
### Form, Array
```javascript
import React, { useState } from 'react';

const App = () => {
  const initialState = [
    { id: 1, name: 'Michael', age: 21, status: false },
    { id: 2, name: 'James', age: 27, status: false },
    { id: 3, name: 'John', age: 24, status: false },
    { id: 4, name: 'Robert', age: 29, status: false },
  ];

  const [users, setUsers] = useState(initialState);
  const [name, setName] = useState('');
  const [age, setAge] = useState('');

  const list = users.map(({ id, name, age, status }) => (
    <li key={id}>
      <span>
        Name: {name}, age: {age}
      </span>
      <button onClick={() => onDelete(id)}>Delete</button>
      <button
        style={{ backgroundColor: status ? 'green' : 'red' }}
        onClick={() => onChangeStatus(id)}
      >
        Status
      </button>
    </li>
  ));

  const onDelete = (id) => {
    const newUsers = users.filter((user) => user.id !== id);
    setUsers(newUsers);
  };

  const onReset = () => {
    setUsers(initialState);
  };

  const onChangeStatus = (id) => {
    const newUsers = [...users];
    const index = newUsers.findIndex((user) => user.id === id);
    newUsers[index].status = !newUsers[index].status;

    setUsers(newUsers);
  };

  const addUser = (event) => {
    event.preventDefault();
    setUsers([...users, { id: users.length + 2, name, age, status: false }]);
    setName('');
    setAge('');
  };

  return (
    <div>
      <form>
        <label htmlFor="name">Name</label>
        <input
          onChange={(event) => setName(event.target.value)}
          type="text"
          name="name"
          value={name}
        />
        <label htmlFor="age">Age</label>
        <input
          onChange={(event) => setAge(event.target.value)}
          type="text"
          name="age"
          value={age}
        />
        <button onClick={addUser}>Send</button>
      </form>
      <ul>{list}</ul>
      <button onClick={onReset}>Reset</button>
    </div>
  );
};

export default App;
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
### Lazy initialization
```javascript
import React, { useState } from 'react';

const heavyFunc = (maxNumber, maxPow) => {
  const data = [];
  for (let i = 1; i <= maxNumber; i++) {
    const record = {};
    for (let pow = 1; pow <= maxPow; pow++) {
      record[pow] = Math.pow(i, pow);
    }
    data.push(record);
  }

  return data;
};

const Row = ({ record }) => {
  return (
    <tr>
      {Object.values(record)
        .sort((a, b) => a - b)
        .map((value, index) => (
          <td key={Object.values(record).length - index}>{value}</td>
        ))}
    </tr>
  );
};

const HeaderRow = ({ maxPow }) => {
  const cells = [];
  for (let pow = 1; pow <= maxPow; pow++) {
    cells.push(
      <th key={maxPow - pow} style={{ minWidth: 100 }}>
        ^{pow}
      </th>
    );
  }

  return <tr>{cells}</tr>;
};

const MAX_NUMBER = 30;
const MAX_POW = 5;

export const Table = () => {
  const [data, setData] = useState(() => {
    console.log('useState initialization');

    return heavyFunc(MAX_NUMBER, MAX_POW);
  });

  const [, setTrigger] = useState();
  const rerender = () => setTrigger({})

  const removeFirst = () => {
    setData((prevData) => {
      const [, ...rest] = prevData;
      return rest;
    });
  };

  console.log('Table has been rendered');

  return (
    <div>
      <button onClick={removeFirst}>Remove First Row</button>
      <button onClick={rerender}>Rerender</button>
      <table>
        <thead>
          <HeaderRow maxPow={MAX_POW} />
        </thead>
        <tbody>
          {data.map((record, index) => (
            <Row key={data.length - index} record={record} />
          ))}
        </tbody>
      </table>
    </div>
  );
};
```
### Calculator, functions in useState
```javascript
import { useState } from 'react';

const add = (a, b) => a + b;
const subtract = (a, b) => a - b;
const multiply = (a, b) => a * b;
const divide = (a, b) => a / b;

const onChange = (setter) => {
  return (event) => {
    const { value } = event.target;
    setter(value ? parseInt(value) : 0);
  };
};

const Calculator = () => {
  const [a, setA] = useState(0);
  const [b, setB] = useState(0);
  const [action, setAction] = useState(() => add);
  const [sign, setSign] = useState('+');

  const applyAction = (fn, fnSign) => {
    return () => {
      setAction(() => fn);
      setSign(fnSign);
    };
  };

  return (
    <div>
      <p>
        <input type="number" value={a} onChange={onChange(setA)} />
        <span> {sign} </span>
        <input type="number" value={b} onChange={onChange(setB)} />
        <span> = {action ? action(a, b) : ''}</span>
      </p>
      <p>
        <button onClick={applyAction(add, '+')}>ADD</button>
        <button onClick={applyAction(subtract, '-')}>SUBTRACT</button>
        <button onClick={applyAction(divide, '/')}>DIVIDE</button>
        <button onClick={applyAction(multiply, '*')}>MULTIPLY</button>
      </p>
    </div>
  );
};

export default Calculator;
```
</details>
<br/>

### `useEffect`
❕ The Effect Hook lets you perform side effects in function components.

> If you’re familiar with React class lifecycle methods, you can think of useEffect Hook as componentDidMount, componentDidUpdate, and componentWillUnmount combined.
<br/>
<details><summary><b>Code Example</b></summary>

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

### `useLayoutEffect`
<br/>
<details><summary><b>Code Example</b></summary>

```javascript
import React, { useState, useEffect, useLayoutEffect } from 'react';

const useCharacterPosition = (step) => {
  const [top, setTop] = useState(0);
  const [left, setLeft] = useState(0);

  useEffect(() => {
    const handleKeyDown = (event) => {
      // eslint-disable-next-line default-case
      switch (event.key) {
        case 'ArrowLeft':
          setLeft((prev) => prev - step);
          break;
        case 'ArrowRight':
          setLeft((prev) => prev + step);
          break;
        case 'ArrowUp':
          setTop((prev) => prev - step);
          break;
        case 'ArrowDown':
          setTop((prev) => prev + step);
          break;
      }
    };

    document.addEventListener('keydown', handleKeyDown);

    return () => {
      document.removeEventListener('keydown', handleKeyDown);
    };
  }, [step]);

  return [top, left];
};

const initialStyle = {
  position: 'absolute',
  top: 0,
  left: 0,
  width: 50,
  height: 50,
  backgroundColor: 'red',
};

export const Example = () => {
  const [top, left] = useCharacterPosition(10);
  const [style, setStyle] = useState(initialStyle);

  useLayoutEffect(() => {
    setStyle((prev) => {
      return {
        ...prev,
        top,
        left,
      };
    });
  }, [top, left]);

  return (
    <div>
      <h1>
        [top: {top}. left: {left}]
      </h1>
      <div style={style} />
    </div>
  );
};
```
</details>
<br/>


### `useMemo`
<br/>
<details><summary><b>Code Example</b></summary>

```javascript
import React, { useState, useEffect, useMemo } from 'react';

const useCharacterPosition = (step) => {
  const [top, setTop] = useState(0);
  const [left, setLeft] = useState(0);

  useEffect(() => {
    const handleKeyDown = (event) => {
      // eslint-disable-next-line default-case
      switch (event.key) {
        case 'ArrowLeft':
          setLeft((prev) => prev - step);
          break;
        case 'ArrowRight':
          setLeft((prev) => prev + step);
          break;
        case 'ArrowUp':
          setTop((prev) => prev - step);
          break;
        case 'ArrowDown':
          setTop((prev) => prev + step);
          break;
      }
    };

    document.addEventListener('keydown', handleKeyDown);

    return () => {
      document.removeEventListener('keydown', handleKeyDown);
    };
  }, [step]);

  return [top, left];
};

const initialStyle = {
  position: 'absolute',
  top: 0,
  left: 0,
  width: 50,
  height: 50,
  backgroundColor: 'red',
};

export const Example = () => {
  const [top, left] = useCharacterPosition(10);

  const style = useMemo(() => {
    return {
      ...initialStyle,
      top,
      left
    }
  }, [top, left])
  

  return (
    <div>
      <h1>
        [top: {top}. left: {left}]
      </h1>
      <div style={style} />
    </div>
  );
};
```
</details>
<br/>

### `custom Hooks`
<br/>
<details><summary><b>Code Example</b></summary>

```javascript
import React, { useState } from 'react';

const useCounter = (initialValue = 0, delta = 1) => {
  const [count, setCount] = useState(initialValue);

  const increment = () => {
    setCount((prevValue) => prevValue + delta);
  };

  const decrement = () => {
    setCount((prevValue) => prevValue - delta);
  };

  return [count, increment, decrement];
};

const YearsCounter = ({ initialValue }) => {
  const [count, inc, dec] = useCounter(initialValue);

  return (
    <div>
      <div>Year:</div>
      <div>
        <button onClick={dec}>{'<<'}</button>
        <span style={{margin: 5}}>{count}</span>
        <button onClick={inc}>{'>>'}</button>
      </div>
    </div>
  );
};

const DecadesCounter = ({ initialValue }) => {
  const [count, inc] = useCounter(initialValue, 10);

  return (
    <div>
      <div>Decade: <span onClick={inc}>{count}'s</span></div>
    </div>
  );
};

export const Counters = () => {
  return (
    <div>
      <YearsCounter initialValue={1970} />
      <DecadesCounter initialValue={1970}/>
    </div>
  )
}
```
### useMergeState
```javascript
import React, { useState } from 'react';

const useMergeState = (initialState) => {
  const [state, setState] = useState(initialState);

  const mergeState = (changes) => {
    setState((prevState) => {
      return {
        ...prevState,
        ...changes,
      };
    });
  };

  return [state, mergeState];
};

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

const initialState = {
  firstName: '',
  lastName: '',
  age: 21,
};

const Form = () => {
  const [data, setData] = useMergeState(initialState);

  const clearForm = () => setData(initialState);

  return (
    <div>
      <form>
        <Field
          name="firstName"
          label="First Name"
          value={data.firstName}
          onChange={(firstName) => setData({ firstName })}
        />
        <Field
          name="lastName"
          label="Last Name"
          value={data.lastName}
          onChange={(lastName) => setData({lastName})}
        />
        <Field
          name="age"
          label="Age"
          type="number"
          value={data.age}
          onChange={(age) => setData({age: age ? parseInt(age) : 0})}
        />
      </form>
      <button onClick={clearForm}>Clear</button>
      <div>First Name: {data.firstName}</div>
      <div>Last Name: {data.lastName}</div>
      <div>Age: {data.age}</div>
    </div>
  );
};

export default Form;

```
</details>
<br/>
