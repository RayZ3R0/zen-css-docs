# Browser Integration for UserChrome.js

This document covers how to integrate with Firefox's internal APIs, handle events, and manage browser components.

## Core Browser Components

### Window Management

```javascript
// Access browser services
const { Services } = ChromeUtils.import("resource://gre/modules/Services.jsm");

// Main browser window
let mainWindow = window.browsingContext.topChromeWindow;

// Browser components
let gBrowser = window.gBrowser;
let tabContainer = gBrowser.tabContainer;
```

### Tab Management

```javascript
// Tab operations
const TabOperations = {
  // Create new tab
  createTab(url, options = {}) {
    return gBrowser.addTab(url, {
      triggeringPrincipal: Services.scriptSecurityManager.createNullPrincipal(
        {}
      ),
      ...options,
    });
  },

  // Move tab
  moveTab(tab, index) {
    gBrowser.moveTabTo(tab, index);
  },

  // Pin/unpin tab
  togglePin(tab) {
    tab.pinned ? gBrowser.unpinTab(tab) : gBrowser.pinTab(tab);
  },

  // Tab group operations
  createGroup() {
    const group = document.createElement("tab-group");
    tabContainer.appendChild(group);
    return group;
  },
};
```

### Workspace Integration

```javascript
const WorkspaceManager = {
  // Get current workspace
  getCurrentWorkspace() {
    return document.querySelector(
      "#zen-workspaces-button .subviewbutton[active]"
    );
  },

  // Switch workspace
  switchWorkspace(id) {
    const button = document.querySelector(
      `#zen-workspaces-button .subviewbutton[workspace-id="${id}"]`
    );
    if (button) {
      button.click();
    }
  },

  // Watch workspace changes
  watchWorkspaces(callback) {
    const observer = new MutationObserver((mutations) => {
      for (const mutation of mutations) {
        if (mutation.attributeName === "active") {
          callback(mutation.target);
        }
      }
    });

    const container = document.querySelector("#zen-workspaces-button");
    observer.observe(container, {
      attributes: true,
      attributeFilter: ["active"],
      subtree: true,
    });
  },
};
```

## Event Management

### Window Events

```javascript
// Main window events
const WindowEvents = {
  init() {
    window.addEventListener("load", this.onLoad);
    window.addEventListener("unload", this.onUnload);

    // Delayed startup
    if (gBrowserInit.delayedStartupFinished) {
      this.onStartupFinished();
    } else {
      Services.obs.addObserver(
        this.onStartupFinished,
        "browser-delayed-startup-finished"
      );
    }
  },

  onStartupFinished(subject, topic) {
    if (topic === "browser-delayed-startup-finished" && subject === window) {
      Services.obs.removeObserver(this.onStartupFinished, topic);
      // Initialize
    }
  },
};
```

### Tab Events

```javascript
// Tab event handling
const TabEvents = {
  init() {
    // Tab container events
    gBrowser.tabContainer.addEventListener("TabOpen", this.onTabOpen);
    gBrowser.tabContainer.addEventListener("TabClose", this.onTabClose);
    gBrowser.tabContainer.addEventListener("TabSelect", this.onTabSelect);
    gBrowser.tabContainer.addEventListener("TabPinned", this.onTabPinned);

    // Tab progress
    gBrowser.addTabsProgressListener({
      onStateChange(browser, webProgress, request, flags, status) {
        // Handle loading states
      },
      onProgressChange(browser, webProgress, request, curSelf, maxSelf) {
        // Handle progress
      },
    });
  },

  cleanup() {
    gBrowser.tabContainer.removeEventListener("TabOpen", this.onTabOpen);
    gBrowser.tabContainer.removeEventListener("TabClose", this.onTabClose);
    gBrowser.tabContainer.removeEventListener("TabSelect", this.onTabSelect);
    gBrowser.tabContainer.removeEventListener("TabPinned", this.onTabPinned);
  },
};
```

## Frame Scripts

### Content Communication

```javascript
const ContentManager = {
  init() {
    // Load frame script
    const frameScript =
      "data:application/javascript;charset=utf-8," +
      encodeURIComponent("(" + this.contentScript.toString() + ")();");
    messageManager.loadFrameScript(frameScript, false);

    // Listen for messages
    messageManager.addMessageListener("FromContent", this.onContentMessage);
  },

  contentScript() {
    // Content script code
    addEventListener("click", (event) => {
      // Handle content events
      sendAsyncMessage("FromContent", {
        type: "click",
        data: { x: event.clientX, y: event.clientY },
      });
    });
  },

  // Send message to content
  sendToContent(message) {
    gBrowser.selectedBrowser.messageManager.sendAsyncMessage(
      "FromChrome",
      message
    );
  },
};
```

## State Management

### Tab States

```javascript
const TabStates = {
  // Watch tab states
  watchTabState(tab) {
    const observer = new MutationObserver((mutations) => {
      for (const mutation of mutations) {
        switch (mutation.attributeName) {
          case "pinned":
            this.onPinnedStateChange(tab);
            break;
          case "busy":
            this.onLoadingStateChange(tab);
            break;
          case "selected":
            this.onSelectionStateChange(tab);
            break;
        }
      }
    });

    observer.observe(tab, {
      attributes: true,
      attributeFilter: ["pinned", "busy", "selected"],
    });
  },
};
```

### Group States

```javascript
const GroupStates = {
  // Watch group states
  watchGroupState(group) {
    const observer = new MutationObserver((mutations) => {
      for (const mutation of mutations) {
        if (mutation.attributeName === "collapsed") {
          this.onGroupStateChange(group);
        }
      }
    });

    observer.observe(group, {
      attributes: true,
      attributeFilter: ["collapsed"],
    });
  },
};
```

## Best Practices

1. **Event Handling**

   - Always clean up event listeners
   - Use appropriate event delegation
   - Handle all relevant states

2. **State Management**

   - Use MutationObserver for state changes
   - Implement proper cleanup
   - Handle state transitions

3. **Performance**

   - Batch DOM operations
   - Use appropriate selectors
   - Implement cleanup procedures

4. **Security**
   - Use appropriate principals
   - Validate content messages
   - Handle errors properly

## Common Integration Patterns

### State Observation

```javascript
function observeElement(element, attributes, callback) {
  const observer = new MutationObserver((mutations) => {
    for (const mutation of mutations) {
      if (attributes.includes(mutation.attributeName)) {
        callback(mutation);
      }
    }
  });

  observer.observe(element, {
    attributes: true,
    attributeFilter: attributes,
  });

  return observer;
}
```

### Event Delegation

```javascript
function delegateEvents(parent, selector, event, handler) {
  parent.addEventListener(event, (e) => {
    if (e.target.matches(selector)) {
      handler(e);
    }
  });
}
```

### Message Handling

```javascript
function createMessageHandler(handlers) {
  return (message) => {
    const handler = handlers[message.name];
    if (handler) {
      return handler(message.data);
    }
  };
}
```

Remember to:

1. Clean up resources properly
2. Handle all state changes
3. Implement proper error handling
4. Follow security best practices
