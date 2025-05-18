# State Management for UserChrome.js

This document covers how to manage state, handle configuration, persist data, and implement proper cleanup procedures in Firefox modifications.

## Configuration Management

### Static Configuration

```javascript
const CONFIG = {
  // Feature flags
  enabled: true,
  debug: false,

  // Timing settings
  timeout: 1000,
  debounceDelay: 250,

  // UI preferences
  styleVariant: "default",
  animations: true,

  // API settings
  endpoint: "http://localhost:8080",
  apiKey: null,
};
```

### Preference Integration

```javascript
// Define preferences
const PREFS = {
  ENABLED: "extensions.mymod.enabled",
  DEBUG: "extensions.mymod.debug",
  STYLE: "extensions.mymod.style",
};

// Access preferences
function getPrefs() {
  return {
    enabled: Services.prefs.getBoolPref(PREFS.ENABLED, true),
    debug: Services.prefs.getBoolPref(PREFS.DEBUG, false),
    style: Services.prefs.getStringPref(PREFS.STYLE, "default"),
  };
}

// Watch for changes
function watchPrefs() {
  Services.prefs.addObserver(PREFS.ENABLED, onPrefsChanged);
  Services.prefs.addObserver(PREFS.STYLE, onPrefsChanged);
}
```

## Data Persistence

### File-based Storage

```javascript
const StorageManager = {
  // Get storage file
  getStorageFile() {
    const file = Services.dirsvc.get("ProfD", Ci.nsIFile);
    file.append("chrome");
    file.append("mymod");
    file.append("data.json");
    return file;
  },

  // Save data
  async saveData(data) {
    const file = this.getStorageFile();
    const json = JSON.stringify(data, null, 2);

    try {
      const encoder = new TextEncoder();
      await IOUtils.writeUTF8(file.path, json);
    } catch (ex) {
      Cu.reportError("Failed to save data: " + ex);
    }
  },

  // Load data
  async loadData() {
    const file = this.getStorageFile();

    try {
      const json = await IOUtils.readUTF8(file.path);
      return JSON.parse(json);
    } catch (ex) {
      Cu.reportError("Failed to load data: " + ex);
      return null;
    }
  },
};
```

### Session Storage

```javascript
const SessionState = {
  // Store in window
  _store: new Map(),

  // Session data methods
  set(key, value) {
    this._store.set(key, value);
  },

  get(key, defaultValue = null) {
    return this._store.get(key) ?? defaultValue;
  },

  delete(key) {
    this._store.delete(key);
  },

  clear() {
    this._store.clear();
  },
};
```

## State Management

### Module State

```javascript
const FeatureModule = {
  // Internal state
  _state: {
    initialized: false,
    active: false,
    data: null,
  },

  // State getters
  get isInitialized() {
    return this._state.initialized;
  },

  get isActive() {
    return this._state.active;
  },

  // State mutations
  setState(updates) {
    this._state = {
      ...this._state,
      ...updates,
    };
    this._notifyStateChanged();
  },

  // State change notifications
  _listeners: new Set(),

  addStateListener(callback) {
    this._listeners.add(callback);
  },

  removeStateListener(callback) {
    this._listeners.delete(callback);
  },

  _notifyStateChanged() {
    for (const listener of this._listeners) {
      try {
        listener(this._state);
      } catch (ex) {
        Cu.reportError(ex);
      }
    }
  },
};
```

### Feature Flags

```javascript
const Features = {
  // Feature definitions
  FLAGS: {
    NEW_UI: "new_ui_enabled",
    BETA_FEATURE: "beta_feature_enabled",
  },

  // Check if feature is enabled
  isEnabled(flag) {
    return Services.prefs.getBoolPref(
      `extensions.mymod.features.${flag}`,
      false
    );
  },

  // Enable/disable feature
  setEnabled(flag, value) {
    Services.prefs.setBoolPref(`extensions.mymod.features.${flag}`, value);
  },
};
```

## Error Handling

### Error Types

```javascript
// Custom errors
class ConfigError extends Error {
  constructor(message) {
    super(message);
    this.name = "ConfigError";
  }
}

class StateError extends Error {
  constructor(message) {
    super(message);
    this.name = "StateError";
  }
}

// Error handling
function handleError(error) {
  // Log error
  Cu.reportError(error);

  // Notify user if needed
  if (error instanceof ConfigError) {
    showNotification("Configuration error", error.message);
  }

  // Reset state if needed
  if (error instanceof StateError) {
    resetState();
  }
}
```

### Recovery Procedures

```javascript
const Recovery = {
  // Backup state
  backupState() {
    const state = FeatureModule._state;
    SessionState.set("backup", state);
  },

  // Restore state
  async restoreState() {
    const backup = SessionState.get("backup");
    if (backup) {
      await FeatureModule.setState(backup);
      return true;
    }
    return false;
  },

  // Reset to defaults
  async resetToDefaults() {
    SessionState.clear();
    await FeatureModule.setState({
      initialized: false,
      active: false,
      data: null,
    });
  },
};
```

## Cleanup Procedures

### Resource Cleanup

```javascript
const Cleanup = {
  // Track resources
  _resources: new Set(),

  // Add resource
  track(resource) {
    this._resources.add(resource);
  },

  // Clean single resource
  async cleanResource(resource) {
    try {
      await resource.cleanup();
      this._resources.delete(resource);
    } catch (ex) {
      Cu.reportError(ex);
    }
  },

  // Clean all resources
  async cleanAll() {
    const resources = Array.from(this._resources);
    await Promise.all(resources.map((r) => this.cleanResource(r)));
  },
};
```

### Window Cleanup

```javascript
// Cleanup on window unload
window.addEventListener("unload", () => {
  // Clean up listeners
  FeatureModule._listeners.clear();

  // Clean up state
  SessionState.clear();

  // Clean up resources
  Cleanup.cleanAll();

  // Remove preferences observers
  Services.prefs.removeObserver(PREFS.ENABLED, onPrefsChanged);
});
```

## Best Practices

1. Always implement proper cleanup
2. Handle errors gracefully
3. Backup state before risky operations
4. Use appropriate storage mechanisms
5. Implement state change notifications
6. Validate state mutations
7. Provide recovery mechanisms
8. Clean up on window unload

## Common Patterns

### State Validation

```javascript
function validateState(state) {
  // Check required fields
  const required = ["id", "type", "data"];
  for (const field of required) {
    if (!(field in state)) {
      throw new StateError(`Missing required field: ${field}`);
    }
  }

  // Validate values
  if (typeof state.id !== "string") {
    throw new StateError("ID must be a string");
  }

  return true;
}
```

### State Persistence

```javascript
async function persistState() {
  const state = FeatureModule._state;

  // Validate state
  if (!validateState(state)) {
    throw new StateError("Invalid state");
  }

  // Backup current state
  Recovery.backupState();

  // Save state
  await StorageManager.saveData(state);
}
```

Remember to implement proper state management to ensure your modifications remain stable and recoverable.
