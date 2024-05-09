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

### New Permissions

None.

### Manifest File Changes

None.

### Abuse Mitigations

Any abuse angle would already be possible by injecting custom CSS, which any extension can do today with host permissions. No new mitigation is warranted

## Alternatives

### Existing Workarounds

- For cases where the extension user owns the site, they can use add additional classes that have a higher priority than those using the browser defined environment variable.
- For arbitrary websites, the same can be accomplished, but it would require parsing the CSS rules on the page (which would be quite costly).

### Open Web API

- There won't likely be an Open Web API for this as it is intended be set by the browser. There may be a WebDriver based option to set these values. There is not one today, however.

## Future Work

`browser.css` could be expanded to do more advanced CSS interactions, perhaps building on top of the [CSS Houdini APIs](https://developer.mozilla.org/docs/Web/CSS/CSS_Houdini).
