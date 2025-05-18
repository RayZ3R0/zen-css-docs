# UI Modifications for UserChrome.js

This document covers how to modify Firefox's user interface through CSS injection, XUL elements, animations, and state handling.

## CSS Integration

### Style Injection

```javascript
// Inject styles into browser
function injectStyles(css) {
  const style = document.createElement("style");
  style.setAttribute("type", "text/css");
  style.textContent = css;
  document.head.appendChild(style);
  return style;
}

// Clean up on unload
let styleSheet = null;
window.addEventListener("unload", () => {
  if (styleSheet) {
    styleSheet.remove();
  }
});
```

### Core CSS Variables

```css
:root {
  /* Theme colors */
  --zen-primary-color: #a0d490;
  --zen-colors-tertiary: rgba(160, 212, 144, 0.1);
  --zen-branding-bg-reverse: var(--identity-tab-color, rgb(43, 42, 51));

  /* Layout measurements */
  --zen-element-separation: 8px;
  --nebula-border-radius: 4px;
  --sidebar-width: 260px;

  /* Settings backgrounds */
  --zen-settings-secondary-background: rgba(249, 249, 250, 0.1);
  --zen-colors-border-contrast: rgba(249, 249, 250, 0.1);
}
```

### Theme Integration

```css
/* Light/Dark theme support */
:root[data-theme="dark"] {
  --zen-bg: rgba(255, 255, 255, 0.1);
  --zen-text: rgb(255, 255, 255);
  --zen-border: rgba(255, 255, 255, 0.1);
}

:root[data-theme="light"] {
  --zen-bg: rgba(0, 0, 0, 0.1);
  --zen-text: rgb(0, 0, 0);
  --zen-border: rgba(0, 0, 0, 0.1);
}
```

## Core Components

### Main Browser Structure

```css
#zen-main-app-wrapper {
  /* Core browser wrapper */
}

#browser {
  /* Main browser area */
}

.browserStack {
  /* Browser stack content */
}

#tabbrowser-tabpanels {
  /* Tab panels container */
}

.browserSidebarContainer {
  /* Sidebar container */
}
```

### Tab System

```css
#tabbrowser-tabs {
  /* Tab container styles */
}

.tabbrowser-tab {
  /* Individual tab */
  &[selected] {
    /* Selected state */
  }
  &[busy] {
    /* Loading state */
  }
  &[pending] {
    /* Pending load */
  }
  &[pinned] {
    /* Pinned state */
  }
  &[muted] {
    /* Audio muted */
  }
  &[soundplaying] {
    /* Audio playing */
  }
}
```

### URL Bar

```css
#urlbar {
  &[open] {
    /* Open state */
  }
  &[focused] {
    /* Focus state */
  }
}

#urlbar-input-container {
  /* Input container */
}

#urlbar-background {
  /* URL bar background */
}
```

## Animations

### Standard Timing

```css
/* Duration values */
:root {
  --animation-fast: 0.2s;
  --animation-standard: 0.3s;
  --animation-smooth: 0.35s;
  --animation-elaborate: 0.5s;
}

/* Easing functions */
:root {
  --ease-standard: ease;
  --ease-natural: cubic-bezier(0.175, 0.885, 0.32, 1.35);
  --ease-resistive: cubic-bezier(0.175, 0.385, 0.12, 1);
  --ease-bounce: cubic-bezier(0.2, 0.8, 0.4, 1);
}
```

### Common Animations

```css
/* Fade in */
@keyframes fadeIn {
  from {
    opacity: 0;
    filter: blur(5px);
  }
  to {
    opacity: 1;
    filter: none;
  }
}

/* Pulse effect */
@keyframes pulse {
  0% {
    transform: scale(0.85);
  }
  50% {
    transform: scale(1.15);
  }
  100% {
    transform: scale(1);
  }
}

/* Loading gradient */
@keyframes gradient-move {
  0% {
    background-position: 0% 50%;
  }
  100% {
    background-position: 300% 50%;
  }
}
```

### Performance Optimizations

```css
/* Hardware acceleration */
.hardware-accelerated {
  transform: translate3d(0, 0, 0);
  backface-visibility: hidden;
  will-change: transform, opacity;
}

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.001ms !important;
    transition-duration: 0.001ms !important;
  }
}
```

## State Management

### Browser States

```css
:root {
  /* Layout states */
  &[zen-compact-mode="true"] {
    /* Compact mode */
  }
  &[zen-right-side="true"] {
    /* Right-side layout */
  }
  &[zen-split="true"] {
    /* Split view active */
  }

  /* Special states */
  &[customizing] {
    /* Customization mode */
  }
  &[inDOMFullscreen] {
    /* Fullscreen mode */
  }
}
```

### Feature States

```css
/* Split view */
#tabbrowser-tabpanels[zen-split-view="true"] {
  .browserSidebarContainer {
    &.deck-selected {
      /* Active split panel */
    }
  }
}

/* Workspace states */
#zen-workspaces-button {
  .subviewbutton {
    &[active] {
      /* Active workspace */
    }
  }
}
```

## Layout Management

### Flexible Layouts

```css
/* Split view layout */
.browserSidebarContainer[zen-split="true"] {
  display: flex;
  width: 50%;
}

/* Tab group layout */
tab-group {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: var(--zen-element-separation);
}
```

### Responsive Handling

```css
/* Compact mode adjustments */
:root[zen-compact-mode="true"] {
  --zen-element-separation: 4px;

  .tab-label {
    font-size: 0.9em;
  }
}

/* Right-side layout */
:root[zen-right-side="true"] {
  .browserSidebarContainer {
    order: 2;
  }
}
```

## Best Practices

1. **State Management**

   - Use zen-\* attributes for core states
   - Follow state hierarchy for specificity
   - Handle all possible state combinations

2. **Animation Performance**

   - Use hardware-accelerated properties
   - Implement proper transition timing
   - Consider reduced motion preferences

3. **Theme Compatibility**

   - Support light/dark themes
   - Use CSS variables for colors
   - Handle transparency properly

4. **Layout Flexibility**

   - Support all viewing modes
   - Handle responsive layouts
   - Maintain proper spacing

5. **Accessibility**
   - Implement proper focus states
   - Support high contrast modes
   - Handle screen reader compatibility
