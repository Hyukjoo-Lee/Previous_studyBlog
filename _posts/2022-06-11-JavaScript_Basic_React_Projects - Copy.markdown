---
layout: default
title: "JavaScript: Basic React Projects"
date: 2022-06-11 11:20:00
categories: JavaScript
---

## Basic projects using concepts I have learned

### 1. ToDo List

#### What I learned
- Three dots(...) in JavaScript : is called the Spread Syntax or Spread Operator. 
    - This allows an iterable such as an array expression 
    - ...array => means all elements in an array
- Do not modify the status directly. Use a function that modifies the state; State must always be 'New'!
- The map function takes all elements of the array and converts them into a new array.
    - If there are six elements in the array, a function is executed six times.
- ReactJS basically recognizes all of elements in the array, so you have to give them unique keys
    - The first argument = value
    - The second is index.
          ```
          toDos.map(item, index) =>
            <li key={index}>{item}</li>
          ```

### Source Code
```javascript
import { useState } from "react";

function App() {

  const [toDo, setToDo] = useState("");
  const [toDos, setToDos] = useState([]);
  const onChange = (event) => setToDo(event.target.value);

  const onSubmit = (event) => {
    event.preventDefault();
    if (toDo === "") {
      return;
    }
    // In normal js, we would use toDos.push when we want to add an element to an array
    // But, remember we never modify a state directly!! We use a function which modifies a state
    // state should be always 'NEW'
    setToDos(currentArray => [toDo, ...currentArray]);
    setToDo("");
  };

  return (
    <div>
      <h1>My To Dos ({toDos.length})</h1>
      <form onSubmit={onSubmit}>
        <input
          onChange={onChange}
          value={toDo}
          type="text"
          placeholder="Write what to do..."
        />
        <button>Add To Do</button>
      </form>
      <hr />
      <ul>
        {
          // map function does take all array's elements and transform them; transforms to a New array
          toDos.map((item, index) =>
            (<li key={index}>{item}</li>)
          )}
      </ul>
    </div>
  );
}

export default App;
```

### 2. Coin Tracker

#### What I learned
- Fetching API and get the data in json format
- Getting selected value using onChange; 원하는 값을 value={} 에 대입
- handleInput


```javascript
import { useState, useEffect } from "react";

function App() {
  const [loading, setLoading] = useState(true);
  const [coins, setCoins] = useState([]);
  const [cost, setCost] = useState(1);
  const [amount, setAmount] = useState(1);
  

  const onChange = (event) => {
    setCost(event.target.value);
  }

  const handleInput = (event) => {
    setAmount(event.target.value);
  }

  useEffect(() => {
    fetch("https://api.coinpaprika.com/v1/tickers?limit=10")
      .then((response) => response.json())
      .then((json) => {
        setCoins(json);
        setLoading(false);
      });
  }, []);

  return (
    <div>
      <h1>The coins! {loading ? "" : `${coins.length} coins here`}</h1>
      {loading ? <strong>loading...</strong> : <select onChange={onChange}>
        <option>Select Coin!</option>
        {coins.map((coin, index) =>
          <option
            key={index}
            value={coin.quotes.USD.price}
            id={coin.symbol}
            symbol={coin.symbol} >
            {coin.name}({coin.symbol}) : {coin.quotes.USD.price} USD
          </option>
        )}
      </select>}
      <h2>Please enter the amount</h2>
      <div>
        <input type="number" value={amount} onChange={handleInput} />$
      </div>
      <h2>You can get {amount / cost} {symbol}</h2>
    </div>);
}
export default App;
```