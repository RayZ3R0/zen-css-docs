# Zen Browser State Management and Special Features

This document provides a detailed analysis of Zen Browser's state management system and special features. This reference is specifically designed for AI models to understand how different states interact and how to implement new features correctly.

## Core States

### Browser States

```css
/* Main browser states and their selectors */
:root {
  /* Theme states */
  &[data-theme="light"] {
    /* Light theme active */
  }
  &[data-theme="dark"] {
    /* Dark theme active */
  }

  /* Layout states */
  &[zen-compact-mode="true"] {
    /* Compact mode active */
  }
  &[zen-right-side="true"] {
    /* Right-side layout active */
  }

  /* Special states */
  &[customizing] {
    /* Customization mode active */
  }
  &[inDOMFullscreen] {
    /* Fullscreen mode active */
  }
}
```

### Tab States

```css
.tabbrowser-tab {
  /* Core states */
  &[selected] {
    /* Currently selected tab */
  }
  &[visuallyselected] {
    /* Visually selected tab */
  }
  &[pending] {
    /* Loading pending */
  }
  &[busy] {
    /* Currently loading */
  }
  &[pinned] {
    /* Pinned tab */
  }
  &[muted] {
    /* Audio muted */
  }
  &[soundplaying] {
    /* Audio playing */
  }

  /* Custom states */
  &[zen-glance-tab] {
    /* Glance view active */
  }
  &[usercontextid] {
    /* Container tab */
  }
}
```

## Feature Management

### Split View System

```css
/* Split view container */
#tabbrowser-tabpanels[zen-split-view="true"] {
  /* Split view layout handling */
  .browserSidebarContainer {
    /* Container states */
    &.deck-selected {
      /* Active split panel */
    }
  }
}

/* Split view tab groups */
tab-group[split-view-group="true"] {
  /* Tab group states */
  :has(.tabbrowser-tab[selected="true"]) {
    /* Contains active tab */
  }
}
```

### Workspace System

```css
#zen-workspaces-button {
  /* Workspace states */
  .subviewbutton {
    &[active] {
      /* Active workspace */
    }
    &:hover {
      /* Hover interactions */
    }
    &:active {
      /* Click state */
    }
  }
}

/* Workspace indicator */
#zen-current-workspace-indicator-container {
  /* Visibility controlled by nebula-remove-workspace-indicator */
}
```

## State Interactions

### URLbar State Management

```css
#urlbar {
  /* Core states */
  &[open] {
    /* Opened state */
  }
  &[focused] {
    /* Focus state */
  }

  /* Interaction states */
  :has(#urlbar-input:focus) {
    /* Input focused */
  }
  :has(.urlbarView-row[selected]) {
    /* Result selected */
  }
}
```

### Sidebar State Management

```css
#sidebar-box {
  /* States based on content */
  :has(#zen-sidebar-web-panel) {
    /* Web panel active */
  }
  :has(.sidebar-panel) {
    /* General panel active */
  }

  /* Visibility states */
  [zen-user-show=""] {
    /* User-triggered visibility */
  }
  [zen-has-hover="true"] {
    /* Hover state active */
  }
}
```

## Special Features

### Container Integration

```css
/* Container tab styling */
.tabbrowser-tab[usercontextid] {
  /* Container identity handling */
  --identity-tab-color: [color]; /* Container color */

  .tab-background::after {
    /* Container visual effects */
    background: linear-gradient(
      45deg,
      var(--identity-tab-color) 30%,
      transparent 70%
    );
  }
}
```

### Custom Attributes

```css
/* Browser-specific attributes */
[zen-compact-mode]
[zen-right-side]
[zen-split-view]
[zen-user-show]
[zen-has-hover]
[zen-glance-tab]
```

## State Transitions

### Visual State Changes

```css
/* State transition patterns */
.state-transition-element {
  /* Base state */
  opacity: 0;
  transform: scale(0.95);

  /* Active state */
  &[active] {
    opacity: 1;
    transform: scale(1);
  }

  /* Transition definition */
  transition: opacity 0.3s ease, transform 0.3s ease;
}
```

### Conditional States

```css
/* State-dependent styling */
.conditional-element {
  /* Parent state dependent */
  :has([selected]) & {
    /* Parent has selected child */
  }

  /* Sibling state dependent */
  &:has(+ .element[active]) {
    /* Next sibling is active */
  }

  /* Child state dependent */
  &:has(> .child[busy]) {
    /* Has busy child */
  }
}
```

## Implementation Guidelines

### 1. State Management Best Practices

- Use appropriate attribute selectors for state changes
- Maintain state consistency across related elements
- Implement proper state cleanup
- Handle state transitions smoothly

### 2. State Hierarchy

```css
/* Priority order for states */
.element {
  /* Base styles */

  /* 1. Core states */
  &[selected] {
  }

  /* 2. Custom states */
  &[zen-custom-state] {
  }

  /* 3. Interaction states */
  &:hover {
  }
  &:active {
  }

  /* 4. Combined states */
  &[selected][zen-custom-state] {
  }
}
```

### 3. State Compatibility

```css
/* State compatibility handling */
.feature {
  /* Base feature styling */

  /* Compact mode compatibility */
  :root[zen-compact-mode="true"] & {
  }

  /* Split view compatibility */
  :root[zen-split-view="true"] & {
  }

  /* Theme compatibility */
  :root[data-theme="dark"] & {
  }
}
```

### 4. Performance Considerations

- Use efficient selectors for state changes
- Minimize DOM queries in state-dependent selectors
- Optimize state transition animations
- Consider state change impact on layout

### 5. Accessibility States

```css
/* Accessibility-related states */
.element {
  /* Focus states */
  &:focus-visible {
  }

  /* Keyboard navigation */
  &:focus-within {
  }

  /* Reduced motion */
  @media (prefers-reduced-motion) {
    /* Simplified transitions */
  }
}
```

## Error Prevention

### 1. State Validation

- Verify state combinations are valid
- Handle invalid state combinations gracefully
- Provide fallback states when necessary

### 2. State Conflicts

```css
/* Conflict resolution */
.element {
  /* State priority handling */
  &[state1][state2] {
    /* Priority rules */
  }

  /* Conflict prevention */
  &:not([state1])[state2] {
    /* Non-conflicting rules */
  }
}
```

### 3. State Debugging

```css
/* Debug helpers */
[invalid-state-combination] {
  outline: 2px solid red !important;
}
```

## Feature Integration

### 1. New Feature Implementation

```css
/* Feature integration pattern */
.new-feature {
  /* Base feature styling */

  /* State integration */
  &[feature-state] {
    /* State-specific styling */
  }

  /* Browser state compatibility */
  :root[zen-state] & {
    /* Browser state handling */
  }
}
```

### 2. Feature State Management

- Define clear state transitions
- Implement proper state cleanup
- Handle feature state conflicts
- Maintain browser state compatibility

### 3. Feature Testing

- Test all state combinations
- Verify state transitions
- Check browser compatibility
- Validate accessibility states
