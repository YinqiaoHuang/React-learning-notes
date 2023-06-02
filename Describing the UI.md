# 🎨 Describing the UI
## 🤔 **What's JSX?**
In React, rendering logic and markup live together in the same place—components. And we write markup with JSX.

**JSX and React are two separate things.** They’re often used together, but you can use them independently of each other. **JSX is a syntax extension, while React is a JavaScript library.**

### 🤓 **The rules of JSX**
#### 1️⃣ **Return a single root element.** 
To return multiple elements from a component, wrap them with a single parent tag such as a `<Fragment></Fragment>`(also can be written as `<> </>`). **JSX looks like HTML, but under the hood it is transformed into plain JavaScript objects.** You can’t return two objects from a function without wrapping them into an array. This explains why you also can’t return two JSX tags without wrapping them into another tag or a Fragment.
#### 2️⃣ **Close all the tags.** 
Self-closing tags like `<img>` must become like `<img src=... />`.
#### 3️⃣ **camelCase most of the things!**
 **JSX turns into JavaScript and attributes written in JSX become keys of JavaScript objects.** So their names can’t contain dashes or be reserved words like `class`. This is why, in React, many HTML and SVG attributes are written in camelCase. For example, instead of `stroke-width` you use `strokeWidth`. Since `class` is a reserved word, in React you write `className` instead, named after the corresponding DOM property. But for historical reasons, **aria-*** and **data-*** attributes are written as in HTML with dashes.

You can use a [converter](https://transform.tools/html-to-jsx) to translate your existing HTML and SVG to JSX.

## 🤔 **How to write JS in JSX?**
- Use `""` to pass strings.
- Use `{}` to pass variables and logics.
They work **inside the JSX tag content** or **immediately after `=` in attributes**.
- `{{ ` and  `}}` is not special syntax: it’s a JavaScript object tucked inside JSX curly braces.


Inline style properties are written in camelCase. For example, HTML `<ul style="background-color: black">` would be written as `<ul style={{ backgroundColor: 'black' }}>`  in your component.
## 🤔 **What's a component?**
React components are **regular JavaScript functions** except:

- Their names always **begin with a capital letter**.
- They **return JSX markup**.

## 🤔 **Importing and Exporting components**
### 📚 **JS grammar: default vs named exports**

Grammar:
![import&export grammar](./img/import%26export%20grammar.png)

A file can have **no more than one default export**, but it can have **as many named exports as you like**.

When you write a default import, you can put any name you want after `import`. In contrast, with named imports, the name has to match on both sides.

### 🤓 **Move a component to another file**
1. Make a new JS file to put the component in.
2. Export your function component from that file (using either default or named exports).
3. Import it in the file where you’ll use the component (using the corresponding technique for importing default or named exports).
## 🤔 **How to build a component?**
### 1️⃣ **Export the component**
### 2️⃣ **Define the function**
Exactly like writing a JavaScript function.

🚨**ATTENTION**: Components' names must start with a capital letter or they won’t work!

### 3️⃣ **Return markup**
Write JSX.

🚨**ATTENTION**: If your markup isn’t all on the same line with the `return` keyword, you must wrap it in `()` or any code on the lines after `return` will be ignored!
## 🤔 **How to nest and organize components?**
Components are regular JavaScript functions, so you can keep multiple components in the same file for sure. This is convenient when components are relatively small or tightly related to each other. If this file gets crowded, you can always move a componet to a separate file.

If A component is rendered inside B component, we can say that B is a parent component, rendering each A as a “child”. 

**Always define every component at the top level.**
## 🤔 **Components all the way down**
Most React apps use components all the way down. This means that you won’t only use components for reusable pieces like buttons, but also for larger pieces like sidebars, lists, and ultimately, complete pages! Components are a handy way to organize UI code and markup, even if some of them are only used once.

Still, many websites only use React to add interactivity to existing HTML pages. They have many root components instead of a single one for the entire page. 

**You can use as much—or as little—React as you need.**

## 🤔 **Passing props to a component**
Every parent component can pass some information to its child components by giving them **props**.

### 🤓 **Give child components some props in two steps**
#### 1️⃣ **Pass props to the child component in parent component**
🌰 
```
export default function Profile() {
  return (
    <Avatar
      person={{ name: 'Lin Lanying', imageId: '1bX5QH6' }}
      size={100}
    />
  );
}
```
#### 2️⃣ **Read props inside the child component**
Props serve the same role as arguments serve for functions—in fact, **props are the only argument to your component**!

2 ways to read props:
```
1️⃣
function Avatar(props) {
  let person = props.person;
  let size = props.size;
  // ...
}
2️⃣ (Mostly used)
// DO NOT forget the curly braces!
function Avatar({ person, size }) {
  // ...
}
```

If you want to give a prop a default value, you can do it in the child component like this:
```
function Avatar({ person, size = 100 }) {
  // ...
}
```
Now, if <Avatar person={...} /> is rendered with no `size` prop, the `size` will be set to 100.

The default value is only used **if the size prop is missing or if you pass size={undefined}**. But if you pass size={null} or size={0}, the default value will not be used.

### 🤓 **Forwarding props with the JSX spread syntax**
🌰
```
function Profile(props) {
  return (
    <div className="card">
      <Avatar {...props} />
    </div>
  );
}
```
### 🤓 **`children` prop in parent component**
When you nest content inside a parent component , it will receive that content in a prop called `children`. You can think of a component with a children prop as having a “hole” that can be “filled in”. You will often use the children prop for visual wrappers: panels, grids, etc. 

🌰
```
import Avatar from './Avatar.js';

function Card({ children }) {
  return (
    <div className="card">
      {children}
    </div>
  );
}

export default function Profile() {
  return (
    <Card>
      <Avatar
        size={100}
        person={{ 
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
    </Card>
  );
}

```
### 🤓 **Don’t try to “change props”**
When a component needs to change its props (for example, in response to a user interaction or new data), it will have to “ask” its parent component to pass it different props—a new object!

## 🤔 **Conditional rendering**
### 1️⃣ **Conditionally returning JSX**
🌰
```
function Item({ name, isPacked }) {
  if (isPacked) {
    return <li className="item">{name} ✔</li>;
  }
  return <li className="item">{name}</li>;
}
```
Return `null` when you don't want to return anything.
### 2️⃣ **Conditionally including JSX**
🌰 **With operator `? : `**
```
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {isPacked ? (
        <del>
          {name + ' ✔'}
        </del>
      ) : (
        name
      )}
    </li>
  );
}
```
🌰 **With operator `&&`**
```
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked && '✔'}
    </li>
  );
}
```
You can read this as “if isPacked, then (&&) render the checkmark, otherwise, render nothing”.

🚨**ATTENTION**: To test the condition, JavaScript converts the left side to a boolean automatically. **However, if the left side is 0, then the whole expression gets that value (0), and React will happily render 0 rather than nothing.**

🌰 **Conditionally assigning JSX to a variable**
```
function Item({ name, isPacked }) {
  let itemContent = name;
  if (isPacked) {
    itemContent = (
      <del>
        {name + " ✔"}
      </del>
    );
  }
  return (
    <li className="item">
      {itemContent}
    </li>
  );
}
```

## 🤔 **Render arrays**
### 🤓 **Render components from an array using JavaScript’s `map()`**
#### 1️⃣ **Store data in an array**
#### 2️⃣ **Use `map()` to turn the array into a new array of JSX nodes**
🌰
```
const listItems = people.map(person => <li>{person}</li>);
```
#### 3️⃣ **Return the new array from your component wrapped in a `<ul>`**
🌰
```
return <ul>{listItems}</ul>;
```

### 🤓 **Use `filter()` to filter the array**
🌰
```
const chemists = people.filter(person =>
  person.profession === 'chemist'
);
```
### 🤓 **Keep list items in order with `key`**
#### 1️⃣ **Why need keys?**
File names in a folder and JSX keys in an array serve a similar purpose. They let us uniquely identify an item between its siblings. A well-chosen key provides more information than the position within the array. Even if the position changes due to reordering, the key lets React identify the item throughout its lifetime.

**You might be tempted to use an item’s index in the array as its key. In fact, that’s what React will use if you don’t specify a key at all. But the order in which you render items will change over time if an item is inserted, deleted, or if the array gets reordered. Index as a key often leads to subtle and confusing bugs.**

**Similarly, do not generate keys on the fly, e.g. with key={Math.random()}. This will cause keys to never match up between renders, leading to all your components and DOM being recreated every time. Not only is this slow, but it will also lose any user input inside the list items. Instead, use a stable ID based on the data.**

**Note that your components won’t receive key as a prop. It’s only used as a hint by React itself. If your component needs an ID, you have to pass it as a separate prop: `<Profile key={id} userId={id} />.`**

#### 2️⃣ **Where to get key?**
Different sources of data provide different sources of keys:

Data from a database: If your data is coming from a database, you can use the database keys/IDs, which are unique by nature.

Locally generated data: If your data is generated and persisted locally (e.g. notes in a note-taking app), use an incrementing counter, `crypto.randomUUID()` or a package like `uuid` when creating items.

#### 3️⃣ **Rules of keys**
Keys must be unique among siblings. However, it’s okay to use the same keys for JSX nodes in different arrays.

Keys must not change or that defeats their purpose! Don’t generate them while rendering.

## 🤔 **Keep components pure**
### 🤓 **Pure function**
- Minds its own business. **It does not change any objects or variables that existed before it was called.** You should not mutate any of the inputs that your components use for rendering. That includes props, state, and context. To update the screen, “set” state instead of mutating preexisting objects.
- **Same inputs, same output.** Given the same inputs, a pure function should always return the same result.

### 🤓 **StrictMode**
React offers a “Strict Mode” in which **it calls each component’s function twice during development**. By calling the component functions twice, Strict Mode helps find components that is not pure.

Strict Mode has no effect in production, so it won’t slow down the app for your users. To opt into Strict Mode, you can wrap your root component into `<React.StrictMode>`. Some frameworks do this by default.

### 🤓 **All communication must happen through `props`**
Remember that React does not guarantee that component functions will execute in any particular order, so you can’t communicate between them by setting variables. All communication must happen through `props`. Rendering can happen at any time, so components should not depend on each others’ rendering sequence.

## ✍️ Practices recomended
Your First Component - 4

Passing Props to a Component - 1, 3

Conditional Rendering - 2, 3

Rendering Lists - 2, 3

Keeping Components Pure - 1, 3