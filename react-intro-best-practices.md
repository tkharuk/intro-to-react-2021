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

## Start simple


```
.
└── /src
    ├── /assets
    ├── /components
    ├── /services
    ├── /store
    ├── /utils
    ├── /pages
    ├── index.js
    └── App.js
```




---

#### Iterate when necessary

<!-- _class: more-space -->
```
.
└── /src
    ├── /common
        ├── /assets
        ├── /components
        ├── /utils
    ├── /Login
        ├── /components
        ├── /services
        ├── index.js
    ├── /Dashboard
        ├── /assets
        ├── /components
        ├── /services
        ├── /store
        ├── /utils
        ├── index.js
    ├── index.js
    └── App.js
```



---

<!-- _class: centered -->
### It applies to most things



---

<!-- _class: centered -->
> [You’ll know when you need Flux.
  If you aren’t sure if you need it - you don’t need it!](https://github.com/petehunt/react-howto#learning-flux)




---

## Be consistent

* There might not be a real benefit of one approach over the other
* But there is a benefit in predictability




---

## Name components

```js
// 👎 Avoid this
export default () => <form>...</form>

// 👍 Name your functions
export default function Form() {
  return <form>...</form>
}
```
<style scoped>p {font-size: 0.8rem;}</style>
It helps when you’re reading an error stack trace and using the React Dev Tools.




---

## Move functions out of Component body

<!-- _class: more-space -->
```js
// 👎 Avoid nesting functions which don't need to hold a closure.
function Component({ date }) {
  function parseDate(rawDate) {
    ...
  }

  return <div>Date is {parseDate(date)}</div>
}

// 👍 Place the helper functions before the component
function parseDate(date) {
  ...
}

function Component({ date }) {
  return <div>Date is {parseDate(date)}</div>
}
```
<style scoped>p {font-size: 0.8rem;}</style>
It helps when you’re reading an error stack trace and using the React Dev Tools.




---

## Especially if those are sub-render functions

<!-- _class: more-space -->
```jsx
// 👎 Don't write nested render functions
function Component() {
  function renderHeader() {
    return <header>...</header>
  }
  return <div>{renderHeader()}</div>
}

// 👍 Extract it in its own component
import Header from '@modules/common/components/Header'

function Component() {
  return (
    <div>
      <Header />
    </div>
  )
}
```
<style scoped>p {font-size: 0.8rem;}</style>
It helps when you’re reading an error stack trace and using the React Dev Tools.




---

## Don't bloat Components

* Average components should not exceed *100-150* lines of code
* Some main pages or controllers would be within *200-300* loc
* It is ok to have even *500* loc for homogeneous code



---

## Don't bloat your props in Components

* A high number of props might mean a component is doing too much
* Average components should not exceed *5-7* props
* Super reusable components might have dozens




---

## Collocate your data in props

```jsx
// 👎 Don't pass values on by one if they're related
<UserProfile
  bio={user.bio}
  name={user.name}
  email={user.email}
  subscription={user.subscription}
/>

// 👍 Use an object that holds all of them instead
<UserProfile user={user} />
```
But only if your data is tightly related!




---

## Assign default props

```jsx
// 👍
function Component({ title = '', tags = [], subscribed = false }) {
  return <div>...</div>
}
<UserProfile user={user} />
```

* Gives hints on expected type
* Prevents undefined errors
* Might look weird on your code coverage report





---

## ALWAYS avoid nested Ternaries 

```jsx
// 👎 Nested ternaries are hard to read in JSX
isSubscribed ? (
  <ArticleRecommendations />
) : isRegistered ? (
  <SubscribeCallToAction />
) : (
  <RegisterCallToAction />
)
```




---

## ALWAYS avoid nested Ternaries 

```jsx
// 👍 Place them inside a component on their own
function CallToActionWidget({ subscribed, registered }) {
  if (subscribed) {
    return <ArticleRecommendations />
  }

  if (registered) {
    return <SubscribeCallToAction />
  }

  return <RegisterCallToAction />
}
```




---

## Write code for humans


> “Any fool can write code that a computer can understand.
   Good programmers write code that humans can understand.”

― [Martin Fowler](https://martinfowler.com/)

---

## Less boilerplate code - easier to read

* use data fetching libraries (e.g. react-query)
* use state management libraries (e.g. rtk, jotai, zustand)




---

## Use absolute paths

```jsx
// 👎 Don't use relative paths
import Input from '../../../../common/components/Input'

// 👍 Absolute ones don't change
import Input from 'common/components/Input'
```

```json
// jsconfig.json
{
  "compilerOptions": {
    "baseUrl": "src"
  },
}
```



---

## Don't optimize prematurely

* Measure
* Fix
* Measure again
* otherwise how can you prove you didn't harm it?


---

## Resources

<style scoped>ul li { font-size: 0.8rem; }</style>
- https://alexkondov.com/tao-of-react
- https://formidable.com/blog/2021/react-components
- https://www.taniarascia.com/react-architecture-directory-structure
- https://github.com/streamich/react-use
- https://github.com/sudheerj/reactjs-interview-questions
- https://www.interviewbit.com/react-interview-questions