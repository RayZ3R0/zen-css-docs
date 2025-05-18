# Zen Browser Workspace Management Documentation

This document details Zen Browser's workspace management system, including button styles, animations, and states. This is designed for AI models to understand and modify workspace-related functionality.

## Core Components

### Workspace Button

```css
#zen-workspaces-button {
  /* Base styling */
  container-type: initial;
  border-radius: calc(var(--nebula-border-radius) - 2px);

  /* Visual effects */
  background-color: light-dark(rgba(255, 255, 255, 0.1), rgba(0, 0, 0, 0.2));
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);

  /* Transitions */
  transition: box-shadow 0.35s ease-out, background-color 0.35s ease-out;
}
```

### Subview Buttons

```css
.subviewbutton {
  /* Core styling */
  border-radius: calc(var(--nebula-border-radius) - 4.5px);
  font-size: 16px;

  /* States */
  opacity: 0.75;
  filter: grayscale(var(--nebula-workspace-grayscale, 100%));

  /* Transitions */
  transition: background-color 0.2s ease, filter 0.3s ease,
    opacity 0.3s ease-out, transform 0.2s ease;
}
```

## Animation System

### Pulse Animation

```css
@keyframes emojiPulse {
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
```

### State Transitions

```css
/* Hover state */
.subviewbutton:hover {
  transform: scale(1.4);
  filter: grayscale(0%);
  opacity: 1;
  z-index: 3;
}

/* Active state */
.subviewbutton:active {
  transform: scale(0.85);
  filter: grayscale(0%);
}

/* Selected state */
.subviewbutton[active] {
  animation: emojiPulse 0.5s ease-out;
}
```

## Style Variations

### Style 0 (Minimal)

```css
@media (-moz-pref("nebula-workspace-style", 0)) {
  #zen-workspaces-button {
    /* Transparent background */
    background-color: transparent;
    box-shadow: none;

    .subviewbutton {
      /* Custom button styling */
      display: flex;
      align-items: center;
      justify-content: center;
      transform-origin: bottom center;
    }
  }
}
```

### Style 2 (Advanced)

```css
@media (-moz-pref("nebula-workspace-style", 2)) {
  #zen-workspaces-button {
    /* Enhanced styling */
    background-color: light-dark(rgba(255, 255, 255, 0.1), rgba(0, 0, 0, 0.2));
    box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);

    .subviewbutton {
      /* Advanced animations */
      transition: transform 0.25s ease, filter 0.3s ease, opacity 0.3s ease-out;
    }
  }
}
```

## State Management

### Button States

```css
/* Default state */
.subviewbutton {
  opacity: 0.75;
  filter: grayscale(100%);
}

/* Hover state */
.subviewbutton:hover {
  opacity: 0.9;
  filter: grayscale(0%);
}

/* Active state */
.subviewbutton[active] {
  opacity: 1;
  filter: grayscale(0%);
}
```

### Visual Indicators

```css
/* Indicator dot */
.subviewbutton::before {
  content: "•";
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -56.5%);
  font-size: 2em;
  transition: opacity 0.3s ease;
}
```

## Workspace Configuration

### Indicator Options

```css
@media (-moz-pref("nebula-remove-workspace-indicator")) {
  #zen-current-workspace-indicator-container {
    display: none;
  }
}
```

### Hover Effects

```css
/* Sibling interactions */
.subviewbutton:hover + .subviewbutton {
  transform: scale(1.15);
  z-index: 2;
  opacity: 0.9;
}

.subviewbutton:hover + .subviewbutton + .subviewbutton {
  transform: scale(1);
  z-index: 1;
  opacity: 0.85;
}
```

## Implementation Guidelines

### 1. State Management

- Handle all button states
- Manage transitions
- Maintain visual feedback
- Consider hover effects

### 2. Animation Performance

```css
/* Efficient transforms */
transform-origin: bottom center;
transform: scale(1);
transition: transform 0.25s ease;
```

### 3. Visual Hierarchy

```css
/* Z-index management */
z-index: 1; /* Base */
z-index: 2; /* Hover siblings */
z-index: 3; /* Hovered item */
```

### 4. Accessibility

```css
/* Clear indicators */
content: "•";
color: light-dark(white, rgba(255, 255, 255, 0.35));
opacity: 1;
```

## Best Practices

### 1. State Transitions

```css
/* Smooth state changes */
transition: transform 0.25s ease, filter 0.3s ease, opacity 0.3s ease-out, color
    0.3s ease-out;
```

### 2. Performance

```css
/* Efficient animations */
transform: scale(1);
transition: transform 0.25s ease;
```

### 3. Visual Feedback

```css
/* Clear state indication */
opacity: 1;
filter: grayscale(0%);
color: inherit;
```

### 4. Error Prevention

```css
/* Handle edge cases */
pointer-events: none;
position: static;
overflow: unset;
```

## Common Patterns

### 1. Hover Effects

```css
element:hover {
  transform: scale(1.4);
  opacity: 1;
  z-index: 3;
}
```

### 2. Active States

```css
element[active] {
  background-color: transparent;
  filter: grayscale(0%);
  opacity: 1;
}
```

### 3. Transitions

```css
transition: transform 0.25s ease, opacity 0.3s ease, filter 0.3s ease;
```

### 4. Visual Indicators

```css
element::before {
  content: "•";
  position: absolute;
  transition: opacity 0.3s ease;
}
```
