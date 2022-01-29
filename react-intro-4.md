---
marp: true
theme: ctheme
html: true
---

<!-- _class: lead -->
<!-- _backgroundColor: #222 -->

# [React.js](https://reactjs.org/)

![width:600px](https://reactjs.org/logo-og.png)

---

## Rules & conventions

* name *components* from capital **L**etter (*e.g. Sidebar*)
* call *hooks* only at top level, unconditionally
* name custom *hooks* with **use** prefix (*e.g. useCurrentUser*)
* don't call *hooks* from regular js functions




---

## One state vs many

```jsx
function Box() {
  const [left, setLeft] = useState(0);
  const [top, setTop] = useState(0);
  const [width, setWidth] = useState(100);
  const [height, setHeight] = useState(100);

  const handleDragDrop = ({ left, top }) =>{
    setLeft(left);
    setTop(top);
  }
  // ...
}
```




---

## One state vs many

```jsx
function Box() {
  const [state, setState] = useState({ left: 0, top: 0, width: 100, height: 100 });

  const handleDragDrop = ({ left,top }) => {
    setState({ left, top });
  }
  // ...
}
```


---

## One state vs many

```jsx
function Box() {
  const [state, setState] = useState({ left: 0, top: 0, width: 100, height: 100 });

  const handleDragDrop = ({ left,top }) =>{
    setState({ left, top }); // state => {left, top}, not {left, top, width, height}
  }
  // ...
}
```




---

## One state vs many

```jsx
function Box() {
  const [state, setState] = useState({ left: 0, top: 0, width: 100, height: 100 });

  const handleDragDrop = ({ left,top }) =>{
    setState(prevState => {...prevState, left, top}); // 👍
  }
  // ...
}
```




---

## One state vs many

```jsx
function Box() {
  const [dimensions, setDimensions] = useState({ width: 100, height: 100});
  const [position, setPosition] = useState({ left: 0, top: 0 });

  const handleDragDrop = ({ left,top }) =>{
    setPosition({ left, top }); // 👍
  }
  // ...
}
```




---

## One state vs many

* primitive `props` and `state` values makes create problems overall
* it is OK to have multiple variables together if they are related
* `setState` replaces the whole value
* multiple `setState` will not, necesarilly, cause multiple re-renders
  * but it will if called from web-api callbacks (e.g. *fetch.then*)
  * might be fixed in [future major React release](https://github.com/reactwg/react-18/discussions/21) under the hood




---

## Stateless vs Stateful

```jsx
const GreetingMessage({ name, title }) => {
    return <h3>Greetings {title}. {name}!</h3>
}
```

* Stateless are often called *dumb, presentational*, etc.




---

## Stateless vs Stateful

```jsx
const EditName = ({ name: oldName, onChange }) => {
  const [name, setName] = useState(oldName);

  const handleSubmit = () => {
    onChange(name);
  };

  const handleChange = (e) => {
    setName(e.target.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={name} onChange={handleChange} />
      <button>Save</button>
    </form>
  );
};
```




---

## Stateless vs Stateful

```jsx
const EditName = ({ name: oldName, onChange }) => {
  const [name, setName] = useState(oldName);

  const handleSubmit = (e) => {
    e.preventDefault(); // <----
    onChange(name);
  };

  const handleChange = (e) => {
    setName(e.target.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={name} onChange={handleChange} />
      <button>Save</button>
    </form>
  );
};
```




---

## Stateless vs Stateful

* Stateless
  * are your lowest, most simple components, similar to `<h1>`
  * own no data, only render it or pass further
* Stateful
  * complex components that handle user interactions, `<select />`
  * own data, might not even render it
  * these days almost everything is stateful




---

## Access previous state from `setState`

```jsx
function App() {
  const [count, setCount] = React.useState(0);

  function delayAddOne() {
    setTimeout(() => {
      setCount(count + 1); // ???
    }, 1000);
  }

  return (
    <>
      <h1>Count: {count}</h1>
      <button onClick={delayAddOne}>+ 1</button>
    </>
  );
}
```




---

## Access previous state

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  const prevCount = /* ?? */;

  return <h1>Now: {count}, before: {prevCount}</h1>;
}
```




---
## Access previous state

```jsx
function Counter() {
  const [count, setCount] = useState(0);
  const prevCount = usePrevious(count);
  return <h1>Now: {count}, before: {prevCount}</h1>;
}

function usePrevious(value) {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  });
  return ref.current;
}
```




---

## Resources

<style scoped>ul li { font-size: 0.8rem; }</style>
- https://reactjs.org/docs/hooks-rules.html
- https://reactjs.org/docs/hooks-faq.html