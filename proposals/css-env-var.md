# Proposal: browser.css.defineEnvVar

## Summary

WebExtension proposal that defines an API that allows extensions to manipulate CSS environmental variables within web pages.

## Document Metadata

Author: @patrickkettner

Sponsoring Browser: TBD

Created: 2024-05-02

## Motivation

### Objective

Developers today do not have a way to set browser defined environment variables. Individual site owners are able to define a [fallback](https://drafts.csswg.org/css-env-1/#env-function), by which they define their own custom property that falls back to a env var, but arbitrarily modifying a value is non trivial and brittle.

### Use Cases

- Design tools could more easily emulate other environments (e.g. mobile devices from desktop, desktop from mobile), and polyfill new/unsupported concepts

### Known Consumers

- [VisBug](https://visbug.web.app/)


## Specification

### Schema

```javascript
interface CssEnvironmentVariables {
  [name: string]: string;
}

declare namespace browser.css {
  function defineEnvVar(envVars: CssEnvironmentVariables): Promise<void>;
}
```

### Behavior

- This API will only be exposed environments that have a CSSOM
- If the value provided is an existing browser provided environment variable, the provided value overwrites it
- Any value provided is attached to a [document root](https://drafts.csswg.org/selectors/#root-pseudo)
- In the case of multiple calls, or multiple extensions setting the same enviromental variable, the most recent value set will overwrite any existing value

### Example

```javascript
browser.css.defineEnvVar({
  "keyboard-inset-height": "50vh",
  "--my-custom-var": "blue"
});
```

This example would define a new CSS environmental variable `--my-custom-var` with the value `blue`, as well as defining/overwriting the browser provieed `keyboard-inset-height` with `50vh`.

