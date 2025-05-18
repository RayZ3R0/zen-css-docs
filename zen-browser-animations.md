# Zen Browser Animation System Documentation

This document provides a detailed analysis of Zen Browser's animation capabilities, transition systems, and dynamic UI behaviors. This is specifically structured for AI models to understand and implement smooth, performant animations.

## Animation Primitives

### Transition Properties

```css
/* Standard transition pattern used throughout Zen Browser */
transition-properties: {
    background-color: true,
    opacity: true,
    transform: true,
    filter: true,
    box-shadow: true
}

/* Common durations */
duration: {
    fast: 0.2s,
    standard: 0.3s,
    smooth: 0.35s,
    elaborate: 0.5s
}

/* Easing functions */
timing-functions: {
    standard: ease,
    natural: cubic-bezier(0.175, 0.885, 0.32, 1.35),
    resistive: cubic-bezier(0.175, 0.385, 0.12, 1),
    bounce: cubic-bezier(0.2, 0.8, 0.4, 1)
}
```

## URL Bar Animations

### Opening Animation

```css
#urlbar[open] {
  /* Base animation sequence */
  animation: fadeIn 0.15s ease-in;

  /* Background transformation */
  #urlbar-background {
    border-radius: 15px;
    backdrop-filter: blur(52px) brightness(0.4) saturate(3.5);
    transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.35);
  }

  /* Content area effects */
  .browserStack {
    filter: blur(1rem) brightness(70%) saturate(180%);
    scale: 1.075;
    transition: all 0.25s cubic-bezier(0.175, 0.385, 0.12, 1);
  }
}
```

## Tab Animations

### Tab State Transitions

```css
.tabbrowser-tab {
  /* Core tab transitions */
  .tab-stack {
    transition: transform 0.2s ease;
  }

  .tab-background {
    transition: background 0.3s ease, box-shadow 0.3s ease, border 0.3s ease;
  }

  /* Loading state animation */
  &[busy] {
    .tab-background::before {
      animation: gradient-move 8s linear infinite;
    }
  }
}
```

### Tab Selection Effects

```css
.tabbrowser-tab[selected] {
  /* Selection animation sequence */
  .tab-background::after {
    transition: opacity 0.3s ease-out;
    animation: selectionFade 0.3s ease-in;
  }
}
```

## Workspace Animations

### Workspace Button Interactions

```css
#zen-workspaces-button {
  .subviewbutton {
    /* Base transitions */
    transition: transform 0.25s ease, filter 0.3s ease, opacity 0.3s ease-out;

    /* Active state animation */
    &[active] {
      animation: emojiPulse 0.5s ease-out;
    }
  }
}
```

## Animation Keyframes

### Core Animations

```css
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

@keyframes gradient-move {
  0% {
    background-position: 0% 50%;
  }
  100% {
    background-position: 300% 50%;
  }
}
```

## Performance Guidelines

### Hardware Acceleration

```css
/* Properties that trigger GPU acceleration */
transform: translate3d(0, 0, 0);
backface-visibility: hidden;
will-change: transform, opacity;
```

### Animation Best Practices

1. **Composite-Only Properties**

   - Prefer `transform` over position properties
   - Use `opacity` for fade effects
   - Avoid animating layout properties

2. **Timing Considerations**

   - Keep animations under 300ms for UI feedback
   - Use longer durations (300-500ms) for elaborate effects
   - Implement proper animation cleanup

3. **Resource Management**
   ```css
   /* Example: Cleanup pattern */
   .animated-element {
     animation: none;
     transition: none;
     transform: none;

     &.active {
       animation: effect 0.3s ease;
     }
   }
   ```

## State-Based Animations

### Loading States

```css
/* Loading animation system */
[busy="true"] {
  /* Progressive loading effects */
  animation: loadingPulse 1.5s ease infinite;

  &::before {
    content: "";
    animation: loadingBar 2s linear infinite;
  }
}
```

### Interactive States

```css
/* Hover state animations */
:hover {
  transition: all 0.3s ease;
  transform: scale(1.05);
}

/* Active state animations */
:active {
  transition: transform 0.1s ease-out;
  transform: scale(0.95);
}
```

## Responsive Animations

### Viewport-Based Adjustments

```css
@media (max-width: 768px) {
  /* Simplified animations for mobile */
  .complex-animation {
    animation-duration: 0.2s;
    transform: none;
  }
}
```

### Reduced Motion

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.001ms !important;
    transition-duration: 0.001ms !important;
  }
}
```

## Animation Patterns

### Common UI Patterns

1. **Fade Transitions**

   ```css
   .fade-pattern {
     opacity: 0;
     transition: opacity 0.3s ease;

     &.visible {
       opacity: 1;
     }
   }
   ```

2. **Scale Transitions**

   ```css
   .scale-pattern {
     transform: scale(0.95);
     transition: transform 0.2s ease;

     &.active {
       transform: scale(1);
     }
   }
   ```

3. **Blur Effects**
   ```css
   .blur-pattern {
     filter: blur(0);
     transition: filter 0.3s ease;

     &.blurred {
       filter: blur(10px);
     }
   }
   ```

## Implementation Guidelines

1. **Animation Chaining**

   - Use transition delays for sequential animations
   - Implement proper state management
   - Consider animation interruption cases

2. **Performance Monitoring**

   - Monitor frame rates during animations
   - Implement fallbacks for lower-end devices
   - Use `will-change` property judiciously

3. **State Management**

   - Clear animation states after completion
   - Handle interrupted animations gracefully
   - Maintain consistent state transitions

4. **Browser Compatibility**
   - Test animations across different browsers
   - Implement appropriate fallbacks
   - Consider vendor prefixes when necessary
