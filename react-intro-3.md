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

## Recap

- `useState` - is a way to retain variable changes in a function
- `useEffect` - is a way to react to variable changes and a place to "communicate" with outer world
- `useEffect` - has a cleanup function called on every new invocation of the hook




---

## Stale state

```jsx
function App() {
  const [count, setCount] = React.useState(0);

  function delayAddOne() {
    setTimeout(() => {
      setCount(count + 1); // will point to old "code"
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

## `useState` set can accept a function

```jsx
function App() {
  const [count, setCount] = React.useState(0);

  function delayAddOne() {
    setTimeout(() => {
      // a function gives access to latest previous value
      // you don't need to rely on "code" in a closure
      setCount(prevCount => prevCount + 1); 
    }, 1000);
  }

  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={delayAddOne}>+ 1</button>
    </div>
  );
}
```


---

<!-- _class: centered -->
<blockquote class="twitter-tweet"><p lang="en" dir="ltr">You can learn more building one autocomplete than building a whole website. Both ways are OK though.</p>&mdash; Dan (@dan_abramov) <a href="https://twitter.com/dan_abramov/status/1395520718033534976?ref_src=twsrc%5Etfw">May 20, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>




---

### Let's build an autocomplete

<style scoped>
img {
  margin-top: 1rem;
  display: flex;
  margin: 0 auto;
}
</style>
![width:900](https://res.cloudinary.com/practicaldev/image/fetch/s--ve6ReKdo--/c_imagga_scale,f_auto,fl_progressive,h_720,q_auto,w_1280/https://rohanfaiyaz.com/img/google-search.png)

<!-- https://restcountries.com/v2/name/ukr?fields=name,flags,alpha3Code -->


---

## Resources

<style scoped>ul li { font-size: 0.8rem; }</style>
- https://www.freecodecamp.org/news/what-every-react-developer-should-know-about-state
- https://www.swyx.io/hooks