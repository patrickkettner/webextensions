# browser.css.defineEnvVar

## Introduction

WebExtension proposal that defines an API that allows extensions to manipulate CSS environmental variables within web pages.

## Interfaces

### The `browser.css` Namespace

The `browser.css` namespace provides methods for interacting with CSS environmental variables.

#### Methods

##### `browser.css.defineEnvVar(details)`

Injects a CSS environmental variable into the page, overwriting any built-in or developer-defined (those beginning with "--") CSS environmental variables.

- `details` (object): An object containing the key-value pair of the environmental variable to define.

### Example

```javascript
browser.css.defineEnvVar({
  "keyboard-inset-height": "50vh",
  "--my-custom-var": "blue"
});
```

This example would define a new CSS environmental variable `--my-custom-var` with the value `blue`, as well as defining/overwriting the browser provieed `keyboard-inset-height` with `50vh`. Developers today do not have a way to set non custom env vars, they can only create new ones that begin with `--`. Any value being injected into the page would be attached to the page's :root. 


## Security and Privacy Considerations

The `browser.css` API must be used with caution, as it allows extensions to manipulate the appearance of web pages. Extensions should only modify CSS environmental variables with the user's explicit consent and for legitimate purposes.
