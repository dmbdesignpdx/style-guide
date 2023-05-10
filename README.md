# Code Style Guidelines and Best Practices

#### The Program

2024.3.11

**[WIP]**

<br>

## Table of Contents

- [HTML Semantics](#html-semantics)
- [Scripts](#scripts)
	- [TypeScript](#typescript)
	- [JavaScript](#javascript)
	- [Comments](#comments)
- [Git Messages](#git-messages)
- [GitHub Branches](#github-branches)
	- [Types of branches](#types-of-branches)
	- [Git flow](#git-flow)
- [JSX](#jsx)
- [Components](#components)
	- [Partials](#partials)
- [Path aliases](#path-aliases)
- [Resources](#resources)

<br>

| Important! | Add the [spell check](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker) extension for VS Code. |
|-|-|

<br>

## HTML Semantics

Structure and order of a document `body`:

```html
<header />
<nav />
<main>
  <!-- Content -->
</main>
<footer />
```

A `section` is a _part_ of something; and an `article` can be its _own_ thing. 

All landmarks need to have an `aria-label` or `aria-labelledby`.

All sectioning content including `header` and `footer` should have a `heading` element.

```html
<section aria-label="A Section of Content">
  <h2>Section Region</h2>
</section>
<article aria-labelledby="one">
  <h2 id="one">Article Region</h2>
</article>
```

| **Note** | `address` is not for physical addresses, just links that are related to the section's subject. |
|-|-|

<br>

## Scripts

Most code styling will be handled and pointed out by ESLint. You can view those rules in the `resources/eslint.json` file of this repo.

### TypeScript

- Never use or allow explicit or implicit `any`
- If you don’t know the type, use `unknown`
- Prefer `type` as much as possible
- Prefer to use path aliases
- Component directories should include an `index.ts` file as a single export for all its files
- Use `enum` for constant dictionaries

<br>

### JavaScript

- Prefer function declarations except for nested callback functions
- Prefer to use named exports over default exports
  - This also applies to React components...
  - Unless framework specific file needs a default export
- Write one variable per declaration
- Use appropriate cases:
    - `CONSTANT_CASE` - Exported constants
    - `camelCase` - Any local variable
    - `PascalCase` - Types, Classes, Components
    - `kebab-case` - Files and directories
- For enums:
    - `PascalCase` name, `CONSTANT_CASE` properties

Function Declarations

```js
// Don't
const MyComponent = () => {};

// Do
function MyComponent {...}

class Animal {
	function speaks() {...}
}

// Callbacks
someList.map(item => {...}); // Ok

window.addEventListener(() => {...}); // Ok

window.addEventListener(function name() { // Ok only if...
	...
	window.removeEventListener(name); // Referenced for removal
});

// React
property={() => void something(val)} // Ok
```

Named exports

```js
// Don't
function MyComponent {...};
export default MyComponent;

// Do
export function MyComponent {...};
```

Constants

```js
// external.js
export const Variations = new Set([...]);
export const Data = new Map([...]);
export const ONE = 1;

// local.js
const two = 2;
const MoreData = new Map([...]);
```

Enums

```ts
enum FontWeight {
  LIGHT = 300,
  NORMAL = 400,
  BOLD = 700,
};
```

```js
// Don't
const three = 3, four = 4;

// Do
const three = 3;
const four = 4;
```

For imports, exports, object and array literals, three or more properties or values get their own line.

```js
// Don't
import { hello, hola, bonjour } from 'module';
const options = { one, two, three };
const numbers = [ 1, 2, 3 ];

// Don't
import { 
  hello, hola, bonjour,
} from 'module';
const options = { 
  one, two, three,
};
const numbers = [ 
  1, 2, 3,
];

// Do
import { 
  hello, 
  hola, 
  bonjour,
} from 'module';
const options = {
  one,
  two,
  three,
};
const numbers = [
  1,
  2,
  3,
];
```

Directories and files

```ts
//
// components/my-component/my-component.tsx
export const MyComponent = () => (...);

//
// components/my-component/index.ts
export * from './my-component';
export * from './types.d';

// So that we can use as...

//
// other-file.tsx
import { MyComponent } from 'comp:my-component';
```

### Comments

Do not be afraid to use comments.

Use JSDoc syntax.

Try to use comments to organize code within files.

### API Changes

Be sure to mark changes with a deprecation notice. Do not remove completely for at least two _minor_ versions.

```ts
// v1.0.0
type Person = {
	firstName: string;
	lastName: string;
	fullName: string;
	age: number;
	
}

// v1.1.0
type Person = {
	firstName: string;
	lastName: string;
	age: number;

	/** @deprecated */
	fullName: string;
}

// v1.3.0
type Person = {
	firstName: string;
	lastName: string;
	age: number;
}
```

<br>

## Git Messages

Use imperative tense:

**"Add feature"**, _not_ "Added feature" or "Adds feature"

```txt
<type>: <summary>
<BLANK LINE>
<body>
<BLANK LINE>
<footer?>
```

- type (see branches below)
- summary
- body: purpose of commit in markdown
- footer: optional, can place useful links here

Top line should have no more than 50 characters.

Body and footer should have no more than 100 characters _per line_.

```md
feat: add underline to nav links

Navigation links are missing an underline to indicated they are links to users.

[ClickUp Task](https://app.clickup.com/t/866an9mzv)
```

The commit message should also be used for your PR title and body.

<br>

## GitHub Branches

Delete your remote branch once merged. (there is an option for this when merging a PR)

### Types of branches

- **feat**: a new feature
- **fix**: bug fixes
- **hotfix**: bug fix with highest priority
- **docs**: adding, updating documentation
- **tests**: adding, updating tests
- **lint**: linting changes
- **refactor**: code changes that are not fixes or features

### Git flow

We intend to keep a linear Git history.

**Merge flow:**

```txt
branch > develop > staging > main
```

This implies that `staging` will always be "ahead" of `main`, `develop` ahead of `staging`, and your `branch` ahead of `develop`. 

Do not create PRs based off of `main` or `staging`; a PR must always be based off of and merged into `develop`.

**Rebase flow:**

```txt
develop < branch
```

`main` and `staging` should _never_ be rebased. If there is an update to `develop`, be sure to rebase your branch on it.

<br>

## JSX

Always provide a `key` for iterated elements or nodes. Do not use the index by itself as the key. Always use the data's `id` if from a CMS; however, if there is no `id`, the index can be combined with some other identifying information as the key.

```jsx
links.map((_, index) => (
  <a key={`main-nav-link-${index}`}></a>
))
```

<br>

## Components

Try to keep components small; they should only have a "single-responsibility".

For example, the nav here has two responsibilities: the logic of the nav itself and the logic of the `nav-menu`.

```jsx
<nav>
  <a href="/">Home</a>
  <div id="nav-menu">
  {links.map((_, index) => (
    <a key={`main-nav-link-${index}`}></a>
  ))}
  </div>
</nav>
```

Moving `nav-menu` to its own component allows for the nav to have a single responsibility.

```jsx
<nav>
  <a href="/">Home</a>
  <NavMenu links={links} />
</nav>
```

### Partials

These are components specific to a parent component and nowhere else. They go in a folder called `_partials`. They do not need their own type files or unit testing because they are only a part of the parent component.

### Directory and Structure

```txt
components
  - header
  	 - _partials
		 		- header-menu.tsx
				- header-section.tsx
		 - header.tsx
		 - header.test.tsx
		 - header.styles.ts
		 - index.ts
		 - types.d.ts
	- footer
	- index.ts
```

<br>

## Path aliases.

Use the following aliases for their respected directories:
- `comp:` - Components
- `types:` - Types
- `const:` - Constants
- `lib:` - Library
- `utils:` - Utilities

Example:

```ts
import { Header } from 'comp:header';
import { Links } from 'const:pages';
import { loaderFunc } from 'utils:network'

...
```

<br>

## Resources

- [HTML](https://html.spec.whatwg.org/multipage/)
- [JSDoc](https://jsdoc.app/)
- [ESLint](https://eslint.org/docs/latest/rules/)
