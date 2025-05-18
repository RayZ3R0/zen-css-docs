# Core Concepts for UserChrome.js Development

This document covers the fundamental concepts and patterns for developing Firefox modifications using UserChrome.js.

## UserScript Metadata

Every uc.js file should start with a metadata block that defines its properties and requirements:

```javascript
// ==UserScript==
// @name          Script Name
// @namespace     http://example.com/
// @description   Description of what the script does
// @include       main
// @compatibility 131+
// @version       YYYY/MM/DD
// @author        Author Name
// ==/UserScript==
```

### Required Metadata Fields:

- `@name`: Script identifier
- `@include`: Where the script runs (usually "main" for browser window)
- `@version`: Use date format for easy tracking
- `@compatibility`: Firefox version compatibility

## Module Patterns

### IIFE Pattern

Most uc.js files use Immediately Invoked Function Expression for encapsulation:

```javascript
(function () {
  // Script contents

  // Public interface
  window.scriptName = {
    publicFunction: function () {},
    publicVariable: value,
  };
})();
```

### ES Modules

For newer scripts, use ES module syntax:

```javascript
import { Services } from "resource://gre/modules/Services.sys.mjs";

export const scriptName = {
  // Module contents
};
```

## Version Handling

### Browser Version Detection

```javascript
// Get major version number
let version = Services.appinfo.version.split(".")[0];

// Version-specific code
if (version >= "131") {
  // Current API usage
} else {
  // Legacy fallback
}
```

### Compatibility Checks

```javascript
// Check for required APIs
if ("URLSearchParams" in window) {
  // Modern code
} else {
  // Fallback implementation
}
```

## File Organization

### Directory Structure

```
userChrome/
├── utils/             # Shared utilities
├── features/          # Main feature scripts
└── resources/         # Additional resources
```

### File Naming

- Use descriptive names: `feature_name.uc.js`
- Helper files: `feature_name_helper.js`
- Style files: `feature_name.css`

## Initialization Patterns

### Standard Initialization

```javascript
// Wait for browser startup
if (gBrowserInit.delayedStartupFinished) {
  init();
} else {
  let delayedStartupFinished = (subject, topic) => {
    if (topic == "browser-delayed-startup-finished" && subject == window) {
      Services.obs.removeObserver(delayedStartupFinished, topic);
      init();
    }
  };
  Services.obs.addObserver(
    delayedStartupFinished,
    "browser-delayed-startup-finished"
  );
}
```

### Resource Loading

```javascript
// Load additional scripts
Services.scriptloader.loadSubScript(
  "chrome://userchrome/content/utils.js",
  window
);
```

## Error Handling

### Basic Error Pattern

```javascript
try {
  // Potentially risky operation
} catch (ex) {
  Services.console.logStringMessage(`Error in ${scriptName}: ${ex.message}`);
}
```

### Cleanup Pattern

```javascript
let cleanup = {
  listeners: new Set(),

  addListener(element, type, fn) {
    element.addEventListener(type, fn);
    this.listeners.add({ element, type, fn });
  },

  shutdown() {
    for (let { element, type, fn } of this.listeners) {
      element.removeEventListener(type, fn);
    }
    this.listeners.clear();
  },
};

// Cleanup on window unload
window.addEventListener("unload", () => cleanup.shutdown());
```

## Code Style Guidelines

### Variable Naming

- Use camelCase for variables and functions
- Use UPPER_CASE for constants
- Prefix private properties with underscore

### Comments and Documentation

```javascript
/**
 * Function description
 * @param {Type} paramName - Parameter description
 * @returns {Type} Return value description
 */
function featureFunction(paramName) {
  // Implementation
}
```

### Modular Organization

```javascript
const FeatureModule = {
  // Configuration
  config: {
    enabled: true,
    timeout: 1000,
  },

  // State
  state: {
    initialized: false,
  },

  // Public methods
  init() {},
  cleanup() {},

  // Private helpers
  _helperFunction() {},
};
```

## Best Practices

1. Always clean up resources and event listeners
2. Use appropriate security checks and permissions
3. Handle version compatibility explicitly
4. Follow Firefox coding style guidelines
5. Document public interfaces and configuration options
6. Use consistent error handling patterns
7. Implement proper initialization and shutdown procedures
8. Keep modifications focused and modular

## Common Pitfalls

1. Not handling browser version changes
2. Memory leaks from unremoved listeners
3. Missing security checks
4. Improper async operation handling
5. Not cleaning up on window unload
6. Hard-coding paths or preferences
7. Not handling edge cases

Remember these core concepts when developing uc.js modifications to ensure robust and maintainable code.
