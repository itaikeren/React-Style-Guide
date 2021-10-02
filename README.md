
# React Style Guide

## Naming conventions
Follow these naming conventions:

**Constants** (`UPPER_CASE`)

```js
const FRONTEND_VERSION = 'development';
```

**Functions** (`camelCase`)

Name functions as self-explanatory as possible. Names of functions that handle user interaction start with handle

```js
function handleShowCategory() {
    // ...
}
```

**Folders** (`kebab-case`)

```
    ├───input-field
    │   ├──InputField.jsx
```

**Files** (`camelCase`)

```
    ├───utils
    │   ├──avgOptionsCalc.js
```

**Component Files** (`PascalCase`)

Name component files with PascalCase, just like components themselves.

```
└───components
    ├───button
    │   ├──Button.jsx
    ├───input-field
    │   ├──InputField.jsx
```

**Boolean Values**

Prefix boolean values with `is`, `has` or `should` (e.g. `isLoading`, `hasTitle` or `shouldShowImage`).

## File Structure
-   **assets**  - Global static assets such as images, svgs, company logo, etc.
-   **components**  - Global shared/reusable *atomic components*, such as button, input field etc.
- **layout** - Almost the same as the components folder, the difference is that components inside the layout reflect part of the page layout that can be reused in multiply pages like footer, header, side-menu
-   **services**  - The connectors to our application with the outside world (reusable API calls etc.)
-   **store**  - Global Redux store
-   **utils**  - Utilities, helpers, constant, and the like
-   **pages**  - The majority of the app would be contained here

```
.
└── /src
    ├── /assets
    ├── /components
    ├── /layout
    ├── /services
    ├── /store
    ├── /utils
    ├── /pages
    ├── index.js
    └── App.js
```

**components folder**

The components folder will only include atomic and stateless components (like button, input-field, drop-down, etc.). Components inside that folder are meant to be reused in multiply pages, the logic of the components should be handle by the page/layout which imports that component.
We should group our components (naming the folder as the component name) and include inside that folder everything related: component, style, tests, storybook
* The component style file should be name as the component name and ending with `style.js`
* The component test file should be name as the component name and ending with `test.js`
* Although it would result in nicer imports, we shouldn't create `index.js` files inside our components folder, nothing worse than looking at your editor trying to find a file when everything is called `index.js`. This is more easily searchable across the codebase and helpful when editing multiple files to avoid confusion

```
└───components
    ├───button
    │   ├───Button.jsx
    │   ├───Button.style.js
    │   ├───Button.test.js
    │   ├───Button.stories.jsx
```

**store folder**

> This folder might need some refactor by a more experienced Redux user

* **actions** - Are functions that are dispatched and then the reducers change the state according to the actions. So all the actions are stored in the actions folder.
* **reducers** – In the reducers folder you store all your reducers which are used to change the state of the application. You have to import all the reducers in the store.js file.
* **store.js** – store.js file contains all the reducers of redux and connects it to the store. You can then connect the app with the redux store.

```
└───store
    ├───actions
    │   ├───authentication
    │   │   ├──authentication.actions.js
    ├───reducers
    │   ├───authentication
    │   │   ├──authentication.reducers.js
    ├───store.js
```

**utils folder**

Global utility functions, like validation and conversion, could easily be used across multiple sections of the app. 

* **constants** - Where you put constant variables which are not to be changed and used by many pages or components. Constants files should end with `constants.js`
* **helpers** - Global utility functions. Helpers files should end with `helpers.js`

```
└───utils
    ├───constants
    │   ├──countries.constants.js
    ├───helpers
    │   ├──validation.helpers.js
    │   ├──currency.helpers.js
```

**pages folder**

Here's where the main part of our app will live: in the  `pages`  folder.

Anything within a page is an item that will likely only be used within that specific page, it might include specific components that won't be global (and are being used only on that page) - an `EmployeeLoginForm`  that will only be used at the  `/employee-dashboard/login`  route, and so on.

The advantage of keeping everything domain-focused instead of putting all the pages components together in  `components/pages`  is that it makes it really easy to look at the structure of the application and know how many top-level pages there are, and know where everything that's only used by that page is. 

* We should consider trying to combine both `il` & `us` components folder into one component and add some logic inside which will control what to show to the user

```
└───pages
    ├───employee-dashboard
    │   ├──register
    │   │   ├──RegisterPage.jsx
    │   │   ├──steps
    │   │   │   ├──Steps.jsx
    │   │   │   ├──Steps.style.js
    │   ...
    ├───investor-dashboard
    │   ...
    ├───website
    │   ...
```

## Aliases
Set up the system to use aliases, so anything within the `components` folder could be imported as `@components`, `assets` as `@assets`, etc. 
eslint: [eslint-plugin-import](https://github.com/benmosher/eslint-plugin-import)

## Components

**Use Function Components**

Write components as functional components using hooks. Only if the desired functionality is not available to function components use class components.

```js
// good
const Button = () => {
    return /* jsx... */;
};
 
// avoid
class Button extends React.Component {
    // ...
}

```

**Enforce Consistent Component Structure**

Write function components with this structure:

```js
// react imports
import React, {useState, useEffect} from 'react'

// external imports
import clsx from 'clsx'

// internal imports
import Button from '@components/button/Button'

// help function that doesn’t explicitly use state and shouldn't recreate every render (and are not belong to the utils folder)
function clacMyNumber(number1, number2){
    // ...
}
 
const MyComponent = ({ prop1, prop2 }) => {
    // declare variables
    const [foo,setFoo] = useState(‘’);
    
    // declare useEffect(s)
    useEffect = () => {
        console.log(‘hello world’)
    } 
 
    // declare event handlers function
    function handleSubmit(){
        // ...
    }
    
    return (
        // ...
    )
};
 
MyComponent.propTypes = {
    // ...
};
 
MyComponent.defaultProps = {
    // ...
};
 
 
export default MyComponent;
```

**Parentheses**

Wrap JSX tags in parentheses when they span more than one line. eslint: [react/jsx-wrap-multilines](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)

```js
// bad
  return <MyComponent variant="long body" foo="bar">
           <MyChild />
         </MyComponent>;
 
// good
  return (
    <MyComponent variant="long body" foo="bar">
      <MyChild />
    </MyComponent>
  );
 
// good, when single line
  const body = <div>hello</div>;
  return <MyComponent>{body}</MyComponent>;
```

**Avoid Uncontrolled Inputs**

Always provide the `value` prop to `<input>`, `<textarea>` or any other component that can be controlled.

## Tags

Always self-close tags that have no children. eslint: [react/self-closing-comp](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

```js
// bad
<Foo variant="stuff"></Foo>
 
// good
<Foo variant="stuff" />
```

If your component has multiline properties, close its tag on a new line. eslint: [react/jsx-closing-bracket-location](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

```js
// bad
<Foo
  bar="bar"
  baz="baz" />
 
// good
<Foo
  bar="bar"
  baz="baz"
/>
```

## Props

* Avoid using an array index as a `key` prop, prefer a stable ID.
* Destructure props inside the parameter parentheses.


## Styling
We should consider switching from Styled-Components to [**TailwindCSS**](https://tailwindcss.com/).
I can make a presentation about it and how to use it, but in a nutshell:

* **Tailwind CSS is an open-source utility-first CSS framework** - Utility-first means that we combine a primitive set of CSS classes to create our desired style instead of semantic styling, where every component or HTML element gets a class name that then contains the style
* **Tailwind CSS follows the Atomic CSS methodology** - In Atomic CSS, every class sets only one styling rule. This way classes are reused more often, which results in a smaller bundle size (in most cases) and they also don't clash with each other. There are some exceptions where a Tailwind class can set multiple rules, but it's usually not the case
* **Convenient class names** - Tailwind out of the box class names are really easy to learn: pb is padding-bottom, mt is margin-top, and so on
* **It’s tiny in production** - Tailwind automatically removes all unused CSS when building for production, which means your final CSS bundle is the smallest it could possibly be. In fact, most Tailwind projects ship less than 10kB of CSS

Here's an example of how it looks like:

```html
<div class="p-6 max-w-sm mx-auto bg-white rounded-xl shadow-md flex items-center space-x-4">
  <div class="flex-shrink-0">
    <img class="h-12 w-12" src="/img/logo.svg" alt="ChitChat Logo">
  </div>
  <div>
    <div class="text-xl font-medium text-black">ChitChat</div>
    <p class="text-gray-500">You have a new message!</p>
  </div>
</div>

```

The example is taken from the official documentation. You can see that there is no CSS code here, just classes all over. These classes are bundled with Tailwind CSS.

**How does TailwindCSS compare to styled-components?**

Styled-components
```js
const Wrapper = styled.div`
padding-bottom: 10px;
@media (min-width: 768px) {
    padding-bottom: 20px;
}
`;

const TestComponent = () => (
    <Wrapper>
        Hello world!
    </Wrapper>
);
```

TailwindCSS
```js
const TestComponent = () => (
    <div className="pb-10 md:pb-20">
        Hello World!
    </div>
);
```
As you can see here, TailwindCSS actually reduces the number of lines of code we have to write to achieve the same goal. This is its whole intention with the utility class approach. It really does simplify writing styled elements. And we even don't talk about the performance boost the application gets.

[Read more about TailwindCSS from their amazing documentation](https://tailwindcss.com/docs)
