# SDE-1 Subjective Test (JavaScript, React, CSS)

## Question 1

```javascript
const [user, setUser] = useState();
user.name = 'Name';
```

What are your observations about the code above? Is it the right practice? If not, explain why.

### Answer: 
The code attempts to modify the `name` property of the `user` state directly. Because `useState()` was called without an initial value, `user` is `undefined`, so this line actually throws `TypeError: Cannot set properties of undefined`. Initialize it instead, e.g. `useState({})`.

However, directly modifying state like this (`user.name = 'Name'`) is not the recommended practice in React.

Instead, you should use the `setUser` function provided by `useState()` to update the state immutably. For example:

```javascript
setUser({ ...user, name: 'Name' });
```

This ensures that React can properly detect and reconcile state changes, leading to more predictable behavior in your application.


## Question 2
What is the output of the following code? Also, give a reason.

```javascript
let company = { name: 'Akshay' };
const organization = [company];
company = null;

console.log(organization);
```

### Answer:
The output for the code will be:

```
[{ name: 'Akshay' }]
```

The reason is that the array stores a reference to the object, not to the variable `company`. Setting `company = null` only rebinds the variable; the object is still referenced from the `organization` array, so it is unchanged (and not garbage-collected).


## Question 3
Give a very short answer. What will be the output of the code below, and why?

```javascript
console.log(a);
console.log(b);
var a = 10;
let b = 12;
```

### Answer:
The output will be:

```
undefined
ReferenceError: Cannot access 'b' before initialization
```

Explanation:
- `console.log(a)`: the `var a` declaration is hoisted to the top of its scope and initialized to `undefined` (the assignment `= 10` stays in place), so it logs `undefined`.
- `console.log(b)`: `b` is declared with `let`, and it's in the temporal dead zone until its declaration is reached, causing a `ReferenceError`.

## Question 4
You are required to show a preview of a website in your React application. How will you achieve this? Also mention the limitations and security concerns.

### Answer
To achieve a preview of a website in a React application, you can use an iframe element. Here's a basic example of how you could implement this:

```jsx
import React from 'react';

const WebsitePreview = ({ url }) => {
  return (
    <div className="website-preview">
      <iframe
        src={url}
        title="Website Preview"
        width="100%"
        height="500px"
        frameBorder="0"
        allowFullScreen
      />
    </div>
  );
};

export default WebsitePreview;
```

In this component:
- `url` prop is passed to specify the URL of the website to preview.
- An iframe is used to embed the website within the React application.

Limitations and security concerns:
- Security: Embedded pages can run scripts, open pop-ups or navigate the top window. Add the `sandbox` attribute (only allow what you need, e.g. `sandbox="allow-scripts"`), only accept `http(s)` URLs (never `javascript:`), and preview only sources you trust.
- Embedding restrictions: Many sites forbid being framed with the `X-Frame-Options: DENY/SAMEORIGIN` header or a `Content-Security-Policy: frame-ancestors` directive, so the iframe stays blank. The usual workaround is a server-side preview (fetch Open Graph metadata or render a screenshot on the backend).
- Same-origin policy: You cannot read or style the content of a cross-origin iframe from your app.
- Performance: Loading large or complex websites into iframes can impact performance, especially on mobile devices.
- Responsive Design: Ensuring that the iframe content is responsive and displays properly across different screen sizes and devices can be challenging.


## Question 5
```jsx
<div id="parent" onClick={this.handleParent)>
  <div id="child" onClick=(this.handleChild)> hello </div>
</div>
```

On clicking the div child which onclick method will be triggered and why?

### Answer
The snippet has two syntax errors: the parent should use `onClick={this.handleParent}` (not `{this.handleParent)`) and the child `onClick={this.handleChild}` (not `(this.handleChild)`). Assuming those are corrected, here's how the event propagation works:

1. If you click on the child div, the event will bubble up through the DOM hierarchy.
2. First, the onClick event of the child div will be triggered.
3. Then, the onClick event of the parent div will be triggered if it's properly defined.

So, both onClick methods will be triggered, first the one for the child and then the one for the parent. This is because of event bubbling in the DOM. To stop the parent handler from running, call `event.stopPropagation()` in `handleChild`.

## Question 6
Why does React not use javascript DOM directly and how does it achieve showing components on DOM? Also, can you manage to make changes to DOM using react, if yes how?

### Answer
React doesn't use JavaScript DOM directly for a few reasons:

1. **Abstraction**: React abstracts away the direct manipulation of the DOM to provide a simpler programming model. This abstraction allows developers to focus on building UI components without worrying about low-level DOM manipulation.

2. **Virtual DOM**: React uses a virtual DOM, which is a lightweight copy of the actual DOM. It reconciles changes made to this virtual DOM with the real DOM, minimizing the number of actual DOM manipulations needed and improving performance.

To show components on the DOM, React mounts a root (`ReactDOM.createRoot(container).render(<App />)` in React 18+, `ReactDOM.render()` in older versions) and calls your components (function components, or `render()` in class components). These methods take JSX (JavaScript XML) or React elements and translate them into DOM elements, which are then inserted into the DOM.

Yes, you can manage changes to the DOM using React. While React encourages you to work with its virtual DOM and JSX syntax, you can still manipulate the DOM directly if needed. React provides a few ways to achieve this:

1. **Refs**: Create a ref with `useRef()` (or `createRef()` in class components) and attach it with the `ref` attribute. `ref.current` is the real DOM node, which you can read or modify, typically inside `useEffect`/`useLayoutEffect`.

2. **Portals**: `ReactDOM.createPortal(children, domNode)` renders children into a DOM node outside the component's parent hierarchy (e.g. modals).

(`ReactDOM.findDOMNode()` used to be another option, but it is deprecated and removed in React 19.)

However, it's generally recommended to avoid direct DOM manipulation when working with React, as it can bypass React's reconciliation process and lead to unexpected behavior. Instead, try to manage state and UI changes through React's component state and props system.


## Question 7
How will you write CSS to achieve the UI below? The numbers are four boxes. Only describe how you would do the layout.

```
        |         2
                 _
1       |           3
                       _
        |           4
```

### Answer
A simple approach: box 1 is a left column spanning the full height, and boxes 2, 3 and 4 are stacked in a right column, separated by a vertical divider (`|`) and horizontal dividers (`_`). With CSS Grid:

```css
.container {
  display: grid;
  grid-template-columns: 1fr 2fr;
  grid-template-rows: repeat(3, 1fr);
}
.box-1 { grid-row: 1 / 4; border-right: 1px solid #000; align-self: stretch; }
.box-2, .box-3 { border-bottom: 1px solid #000; }
```

Alternative described with Flexbox and pseudo-elements:

1. Use a parent container with `display: flex` to create a horizontal layout.
2. Each box (numbered 1-4) will be a child element.
3. Set the width and height of each box accordingly.
4. Use margins or padding to adjust the spacing between the boxes.
5. For the diagonal line (from 2 to 3 and from 3 to 4), you might consider using CSS pseudo-elements (::before or ::after) or background images to create the effect.

Here's a basic example of the CSS:

```css
.container {
  display: flex;
}

.box {
  width: 50px;
  height: 50px;
  border: 1px solid black;
  margin-right: 20px; /* Adjust spacing between boxes */
}

/* Example of pseudo-elements for diagonal lines */
.box:nth-child(2)::after,
.box:nth-child(3)::before {
  content: '';
  position: absolute;
  width: 0;
  height: 0;
  border-style: solid;
}

.box:nth-child(2)::after {
  border-width: 0 0 50px 50px;
  border-color: transparent transparent transparent black;
  top: -50px;
  left: 100%;
}

.box:nth-child(3)::before {
  border-width: 50px 50px 0 0;
  border-color: black transparent transparent transparent;
  top: 100%;
  left: 0;
}
```

Adjust the widths, heights, and spacing values according to your specific design requirements.

## Question 8
What will be the output of the following?

```javascript
setInterval(() => console.log('Inside func'), 1000);
[51, 62, 53, 344].reduce((x, y) => console.log(x, y));
```

### Answer
The output of the given code will be:

```
51 62
undefined 53
undefined 344
Inside func      (after 1 s)
Inside func      (after 2 s, and so on every second)
```

Explanation:
- `setInterval` only schedules its callback, so the synchronous `reduce` runs first. Afterwards 'Inside func' is printed every 1000 milliseconds until `clearInterval` is called.
- The `reduce` function iterates over the array `[51, 62, 53, 344]` and applies the callback function `(x, y) => console.log(x, y)`.
- The `console.log(x, y)` statement inside the `reduce` callback prints the accumulated value `x` and the current array element `y` at each iteration.
- Since no initial value is provided for the accumulator in the `reduce` function, the first element of the array is used as the initial value for `x`.
- The first element (`51`) is printed alongside the second element (`62`), then `undefined` (the return value of `console.log(x, y)`) is printed alongside the third element (`53`), and `undefined` is printed alongside the fourth element (`344`).


## Question 9
What will be the output of the following code? Give a short reason.

```javascript
const func1 = () => console.log('Running 1st function');
const func2 = () => setTimeout(() => console.log('Running 2nd function'));
const func3 = () => console.log('Running 3rd function');

func2();
func1();
func3();
```

### Answer
The output of the code will be:

```
Running 1st function
Running 3rd function
Running 2nd function
```

The reason for this output is that `func2()` uses `setTimeout` which schedules the execution of its callback function asynchronously after a specified delay (in this case, 0 milliseconds). So, `func2()` is executed first, but its console log statement is delayed. Then, `func1()` and `func3()` are executed synchronously, resulting in their console log statements being printed first. Finally, after a short delay, the callback function of `func2()` is executed, printing its console log statement.

## Question 10
How do you pass data between components in a React application? If you had a hundred components, how would you choose to pass data between them?

### Answer
To pass data between components in a React application, you can use props for parent-child communication and context or Redux for communication between deeply nested or unrelated components.

If given a hundred components, I would consider using context or Redux for passing data between them, as these approaches offer a centralized and scalable solution, making data management more organized and efficient across a large number of components.

## Question 11
You are given an API to fetch data. Which React hook would you use to do so? The API will be called again from other parts of the application; which mechanism would you choose to optimise this?

### Answer

To fetch data from an API in a React application, I would use the `useEffect` hook along with the `useState` hook to manage the state of the fetched data. Specifically, I would use `useEffect` with an empty dependency array to make the API call once when the component mounts.

To optimize subsequent calls to the API, I would implement some form of caching mechanism, such as a data-fetching library with a built-in cache (TanStack Query / React Query, SWR, RTK Query), memoization, or caching the data in Redux or context. This way, if the data has already been fetched once, subsequent calls can retrieve the data from the cache instead of making another API call, reducing unnecessary network requests and improving performance.

## Question 12
You are given two scenarios. Suggest the method you would choose to optimise each one.

1. An API response is an array of almost a thousand objects. You must compute a new key for each object and set the state with the result.

2. A  react component that takes a lot of time to load

### Answer
For scenario 1:
Given the task involves modifying each object in a large array and resetting the state, I would choose to optimize using batch processing techniques. Instead of updating the state after each modification, which could lead to performance issues with a large array, I would perform the modifications in batches, update the state once after all modifications are complete. This can be achieved by dividing the array into smaller chunks, processing each chunk asynchronously, and then updating the state with the modified array. Additionally, using techniques like memoization or using efficient algorithms for the modifications can further optimize the process.

For scenario 2:
For a React component that takes a lot of time to load, I would choose to optimize using lazy loading and code splitting techniques. By lazy loading components or parts of the component tree that are not immediately required, we can defer their loading until they are needed, improving the initial load time of the application. Similarly, code splitting allows us to split our code into smaller bundles that can be loaded on demand, reducing the initial bundle size and improving load times. Additionally, optimizing the component's rendering performance by identifying and optimizing any expensive operations or rendering bottlenecks can further improve the overall loading time of the component.

## Question 13
You are given a set of API's. You have to return the response of the API that executes the fastest, Suggest how will you achieve this and also give a reason.

### Answer
Fire all requests at the same time with `fetch` and let the promise combinators pick the winner:

- `Promise.race()` settles with the first promise that settles, whether it succeeds or fails.
- `Promise.any()` resolves with the first promise that succeeds and ignores failures (it rejects only if all of them fail). This is usually what you want.

```javascript
async function fastest(urls) {
  const controller = new AbortController();
  const requests = urls.map((url) =>
    fetch(url, { signal: controller.signal }).then((res) => {
      if (!res.ok) throw new Error(`${url}: ${res.status}`);
      return res.json();
    })
  );
  try {
    return await Promise.any(requests);
  } finally {
    controller.abort(); // cancel the slower requests
  }
}
```

This approach ensures that your frontend application remains responsive while fetching data from multiple APIs, and it optimizes the user experience by displaying the fastest response as soon as it's available.

## Question 14
You are creating a front-end application where you have to store and retrieve the data. How would you achieve the same using just a browser?

Also, the storage you use need not be persisted forever only till the user closes the tab.

### Answer
To store and retrieve data in a browser without persisting it beyond the user's session (i.e., until the user closes the tab), you can use browser storage mechanisms such as sessionStorage or in-memory storage.

Here's how you can achieve this:

1. **Session Storage**: Session storage allows you to store key-value pairs within the current browser tab's session. Data stored in session storage is available only for the duration of the page session, including page reloads and restores.

   ```javascript
   // Storing data in session storage
   sessionStorage.setItem('key', 'value');

   // Retrieving data from session storage
   const data = sessionStorage.getItem('key');
   ```

2. **In-Memory Storage**: You can also store data in-memory within your JavaScript code. This approach is suitable for storing temporary data that doesn't need to persist beyond the current page session.

   ```javascript
   // Define a variable to store data
   let inMemoryData = {};

   // Storing data in-memory
   inMemoryData['key'] = 'value';

   // Retrieving data from in-memory storage
   const data = inMemoryData['key'];
   ```

Both sessionStorage and in-memory storage provide a way to store and retrieve data within the browser tab's session without persisting it beyond the tab's lifetime. Choose the appropriate method based on your specific requirements and the scope of data you need to manage within your front-end application.

## Question 15
What is the text colour of the div in the following code?

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        .box {
            color: blue;
        }
    </style>
</head>
<body>
    <div class="box" style="color: red;">Hello, world!</div>
</body>
</html>
```

### Answer
The text is **red**.

The `<div>` has both an inline style (`color: red;`) and a class-based style (`.box { color: blue; }`). Inline styles have higher specificity than any selector in a stylesheet, so the inline rule wins. Only a stylesheet rule marked `!important` would override it.

---

Feel free to use these questions for your interview preparation or customize them as needed!
