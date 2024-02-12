- [React Summary](#react-summary)
  - [Getting started with React](#getting-started-with-react)
  - [Building components](#building-components)
    - [Creating a component](#creating-a-component)
    - [Rendering a List](#rendering-a-list)
    - [Conditional Rendering](#conditional-rendering)
    - [Handling events](#handling-events)
    - [Defining state](#defining-state)
    - [Props](#props)
    - [Passing Children](#passing-children)
  - [Styling components](#styling-components)
    - [Vanilla CSS](#vanilla-css)
    - [CSS modules](#css-modules)
    - [CSS in JS](#css-in-js)
    - [Inline Styling](#inline-styling)
  - [Managing Component State](#managing-component-state)
    - [Updating Objects](#updating-objects)
    - [Updating nested objects](#updating-nested-objects)
    - [Updating arrays](#updating-arrays)
    - [Updating array of objects](#updating-array-of-objects)
    - [Updating with Immer](#updating-with-immer)


# React Summary

## Getting started with React

- React is a JavaScript library for building dynamic and interactive user interfaces.
- In React applications, we don’t query and update the DOM. Instead, we describe our
  application using small, reusable components. React will take care of efficiently creating
  and updating DOM elements.
- React components can be created using a function or a class. Function-based
  components are the preferred approach as they’re more concise and easier to work
  with.
- JSX stands for JavaScript XML. It is a syntax that allows us to write components that
  combine HTML and JavaScript in a readable and expressive way, making it easier to
  create complex user interfaces.
- When our application starts, React takes a tree of components and builds a JavaScript
  data structure called the virtual DOM. This virtual DOM is different from the actual
  DOM in the browser. It’s a lightweight, in-memory representation of our component
  tree
- When the state or the data of a component changes, React updates the
  corresponding node in the virtual DOM to reflect the new state. Then, it compares
  the current version of virtual DOM with the previous version to identify the nodes
  that should be updated. It’ll then update those nodes in the actual DOM.
- In browser-based apps, updating the DOM is done by a companion library called
  ReactDOM. In mobile apps, React Native uses native components to render the
  user interface.
- Since React is just a library and not a framework like Angular or Vue, we often
  need other tools for concerns such as routing, state management,
  internationalization, form validation, etc

## Building components

In React apps, a component can only return a single element. To return multiple
elements, we wrap them in a fragment, which is represented by empty angle brackets.

- To render a list in JSX, we use the ‘array.map()’ method. When mapping items, each
  item must have a unique key, which can be a string or a number.
- To conditionally render content, we can use an ‘if’ statement or a ternary operator.
- We use the state hook to define state (data that can change over time) in a component. A
  hook is a function that allows us to tap into built-in features in React.
- Components can optionally have props (short for properties) to accept input.
- We can pass data and functions to a component using props. Functions are used to
  notify the parent (consumer) of a component about certain events that occur in the
  component, such as an item being clicked or selected.
- We should treat props as immutable (read-only) and not modify them.
- When the state or props of a component change, React will re-render the component
  and update the DOM accordingly.

### Creating a component

```tsx
const Message = () => {
  return <h1>Hello World</h1>;
};

export default Message;
```

### Rendering a List

```tsx
const Component = () => {
  const items = ["a", "b", "c"];
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{item}</li>
      ))}
    </ul>
  );
};
```

### Conditional Rendering

```tsx
{
  items.length === 0 ? "a" : "b";
}
{
  items.length === 0 && "a";
}
```

### Handling events

```tsx
<button
  onClick={() => {
    console.log("clicked");
  }}
></button>
```

### Defining state

```tsx
const [name, setName] = useState("");
```

### Props

```tsx
interface Props {
  name: string;
}

const Component = ({ name }: Props) => {
  return <p>{name}</p>;
};
```

### Passing Children

```tsx
interface Props {
  children: ReactNode;
}
const Component = ({ children }: Props) => {
  return <div>{children}</div>;
};
```

## Styling components

- We have several options for styling React components, including vanilla CSS, CSS modules, CSS-in-JS, and inline styles.
- With vanilla CSS, we write our component styles in a separate CSS file and import it into the component file. However, we may encounter conflicts if the same CSS classes are defined in multiple files.
- CSS modules resolve this issue by generating unique class names during the build process.
- With CSS-in-JS, we define all the styles for a component alongside its code. Like CSS modules, this provides scoping for CSS classes and eliminates conflicts. It also makes it easier for us to change or delete a component without affecting other components.
- The separation of concerns principle suggests that we divide a program into distinct sections or modules where each section handles a specific functionality. It helps us build modular and maintainable applications.
- With this principle, the complexity and implementation details of a module are hidden behind a well-defined interface.
- Separation of concerns is not just about organizing code into files, but rather dividing areas of functionality. Therefore, CSS-in-JS does not violate the separation of concerns principle as all the complexity for a component remains hidden behind its interface.
- Although inline styles are easy to apply, they can make our code difficult to maintain over time and should only be used as a last resort.
- We can add icons to our applications using the react-icons library.
- There are several UI libraries available that can assist us in quickly building beautiful and modern applications. Some popular options include Bootstrap, Material UI, TailwindCSS, DaisyUI, ChakraUI, and more.

### Vanilla CSS

```tsx
import "./ListGroup.css";

function ListGroup() {
  return <ul className="list-group"></ul>;
}
```

### CSS modules

```tsx
import styles from "./ListGroup.module.css";

function ListGroup() {
  return <ul className={[styles.listGroup, styles.container].join(" ")}></ul>;
}
```

### CSS in JS

- Libraries available for CSS-In-JS styling are

  - Styled components
  - Emotion
  - Polished

- Example using styled-components library

```bash
npm i styled-components
# @types is a library that contains type defintion for variables used in popular JS libraries.
# Useful when using JS libraries without type defined in TS
npm i @types/styled-components
```

```tsx
import styled from "styled-components";
import { useState } from "react";

const [selectedIndex, setSelectedIndex] = useState(-1);

interface Props {
  items: string[];
  heading: string;
}

// Props for styled React Component
interface ListItemProps {
  active: boolean;
}

// styled contains all the HTML elements
// This line returns a react component with the styles we defined
const List = styled.ul`
  list-style: none;
  color: white;
  background: black;
  padding: 0;
`;

// Styled React component with props
const ListItem = styled.li<ListItemProps>`
  padding: 3px 0;
  background: ${(props) => (props.active ? "blue" : "none")};
`;

function ListGroup({ items, heading }: Props) {
  return (
    <List>
      {items.map((item, index) => (
        <ListItem active={index === selectedIndex} key={index}>
          {item}
        </ListItem>
      ))}
    </List>
  );
}
```

### Inline Styling

**NOT RECOMMENDED. USE AS A LAST RESORT ONLY**

Inline styling in React refers to the practice of applying styles directly to individual React elements using the `style` attribute. Here are some key points to note about inline styling in React:

1. **JavaScript Object Syntax:**

   - Inline styles in React use a JavaScript object syntax to define CSS styles.
   - Each style property is represented as a camelCased key in the object.

   ```jsx
   const styleObject = {
     color: "red",
     fontSize: "16px",
     backgroundColor: "#f0f0f0",
   };

   <div style={styleObject}>Hello, React!</div>;
   ```

2. **Dynamic Styles:**

   - Since styles are defined using JavaScript objects, you can dynamically generate styles based on variables or conditions.

   ```jsx
   const dynamicColor = "blue";

   const dynamicStyle = {
     color: dynamicColor,
     fontSize: "18px",
   };

   <div style={dynamicStyle}>Dynamic Style</div>;
   ```

3. **String Interpolation:**

   - String interpolation can be used within style values when necessary.
   - However, it's generally more common and recommended to use JavaScript objects.

   ```jsx
   const fontSizeValue = "20px";

   const dynamicStyle = {
     fontSize: fontSizeValue,
   };

   <div style={dynamicStyle}>Dynamic Font Size</div>;
   ```

4. **Style Prop in JSX:**

   - The `style` attribute is a standard prop in JSX that accepts a JavaScript object for styling.

   ```jsx
   const myStyle = {
     fontWeight: "bold",
     textAlign: "center",
   };

   <div style={myStyle}>Styled Component</div>;
   ```

5. **Avoiding XSS Attacks:**

   - Be cautious when dynamically generating styles from user inputs to avoid XSS attacks.
   - Ensure that user inputs are sanitized and properly validated before applying them as styles.

6. **CSS-in-JS Libraries:**

   - For more advanced styling needs, you may consider using CSS-in-JS libraries like Styled Components or Emotion.
   - These libraries provide a more sophisticated way to handle styles in React applications.

   ```jsx
   import styled from "styled-components";

   const StyledDiv = styled.div`
     color: red;
     font-size: 16px;
   `;

   <StyledDiv>Hello, Styled Components!</StyledDiv>;
   ```

Inline styling is a convenient and flexible way to apply styles directly to React components, especially for smaller applications or components where the use of external stylesheets or CSS-in-JS libraries might be overkill. However, for larger projects, using external stylesheets or CSS-in-JS libraries is often recommended for better maintainability and organization.

## Managing Component State

- The state hook allows us to add state to function components.
- Hooks can only be called at the top level of components.
- State variables, unlike local variables in a function, stay in memory as long as the component is visible on the screen. This is because state is tied to the component instance, and React will destroy the component and its state when it is removed from the screen.
- React updates state in an asynchronous manner, so updates are not applied immediately. Instead, they’re batched and applied at once after all event handlers have
  finished execution. Once the state is updated, React re-renders our component.
- Group related state variables into an object to keep them organized.
- Avoid deeply nested state objects as they can be hard to update and maintain.
- To keep state as minimal as possible, avoid redundant state variables that can be computed from existing variables.
- A pure function is one that always returns the same result given the same input. Pure functions should not modify objects outside of the function.
- React expects our function components to be pure. A pure component should always return the same JSX given the same input.
- To keep our components pure, we should avoid making changes during the render phase.
- Strict mode helps us catch potential problems such as impure components. Starting from React 18, it is enabled by default. It renders our components twice in development mode to detect any potential side effects.
- When updating objects or arrays, we should treat them as immutable objects. Instead of mutating them, we should create new objects or arrays to update the state.
- Immer is a library that can help us update objects and arrays in a more concise and mutable way.
- To share state between components, we should lift the state up to the closest parent component and pass it down as props to child components.
- The component that holds some state should be the one that updates it. If a child component needs to update some state, it should notify the parent component using a callback function passed down as a prop.

### Updating Objects

```tsx
import { useState } from "react";

function App() {
  const [drink, setDrink] = useState({
    title: "cola",
    price: 5,
  });

  return (
    <div>
      <button
        onClick={() => {
          // Only an object with all the properties can be passed to set state objects
          // No partial updates available
          // To prevent typing entire object again and again, one can use spread operator and type only those properties they wish to update
          setDrink({ ...drink, price: 6 });
        }}
      >
        Click Me
      </button>
    </div>
  );
}
export default App;
```

### Updating nested objects

```tsx
import { useState } from "react";

function Employee() {
  const [user, setUser] = useState({
    name: "John",
    address: {
      street: "Baker Street",
      city: "London",
      zipCode: "94111",
    },
  });

  // The more deeper our object structure is, the more complex the update logic becomes
  const handleClick = () => {
    setUser({ ...user, address: { ...user.address, street: "Church Street" } });
  };

  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}
export default App;
```

### Updating arrays

```tsx
import { useState } from "react";

function App() {
  const [tags, setTags] = useState(["happy", "cheerful"]);

  const handleClick = () => {
    // Add a tag called 'exciting'
    setTags([...tags, "exciting"]);

    // Remove cheerful tag
    setTags(tags.filter((tag) => tag !== "cheerful"));

    // Update happy tag to happiness
    setTags(tags.map((tag) => (tag === "happy" ? "happiness" : tag)));
  };
  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}

export default App;
```

### Updating array of objects

```tsx
import { useState } from "react";

function App() {
  const [bugs, setBugs] = useState([
    { id: 1, name: "UI Glitch", fixed: "false" },
    { id: 2, name: "Biometric login failed", fixed: "false" },
  ]);

  const handleClick = () => {
    // Fix the bug 1
    setBugs(
      bugs.map((bug) => (bug.id === 1 ? { ...bug, fixed: "true" } : bug))
    );
  };

  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}

export default App;
```

### Updating with Immer

```tsx
import { produce } from "immer";

function App() {
  const [bugs, setBugs] = useState([
    { id: 1, name: "UI Glitch", fixed: "false" },
    { id: 2, name: "Biometric login failed", fixed: "false" },
  ]);

  const handleClick = () => {
    // Fix the bug 1
    setBugs(
      produce((draft) => {
        const bug = draft.find((bug) => bug.id === 1);
        if (bug) bug.fixed = true;
      })
    );
  };

  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}
```
