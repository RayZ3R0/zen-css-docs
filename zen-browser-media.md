# Zen Browser Media Controls Documentation

This document details all media-related selectors, states, and features in Zen Browser. It's designed for AI models to understand and modify media playback UI components.

## Media Control Components

### Main Toolbar

```css
#zen-media-controls-toolbar {
  /* Main media controls container */
  /* States:
       - .playing (active playback)
    */
}

toolbaritem {
  /* Individual control items */
  /* Supports gradient animations and hover effects */
}
```

### Control Buttons

```css
/* Core Media Controls */
#zen-media-playpause-button
#zen-media-nexttrack-button
#zen-media-previoustrack-button
#zen-media-mute-button
#zen-media-focus-button
#zen-media-close-button

/* Common button properties */ {
  transition: opacity 0.3s ease-in-out, transform 0.17s ease-out;
  /* States: :hover */
}
```

### Progress Bar

```css
#zen-media-progress-bar {
  /* Custom range input styling */
  &::-moz-range-thumb {
  } /* Playhead */
  &::-moz-range-track {
  } /* Track */
  &::-moz-range-progress {
  } /* Progress */
}
```

### Media Info Display

```css
#zen-media-service-hbox  /* Container */
/* Container */
#zen-media-title        /* Title text */
#zen-media-artist      /* Artist text */
#zen-media-info-vbox; /* Info container */
```

## Tab Audio States

### Audio Indicators

```css
.tab-icon-overlay {
    /* States */
    &[soundplaying] /* Audio playing */
    &[muted]        /* Audio muted */
    &[activemedia-blocked] /* Media blocked */
}
```

### Tab States

```css
.tabbrowser-tab {
    /* Media States */
    &[soundplaying]
    &[muted]
    &[activemedia-blocked]
}
```

## Animation System

### Gradient Effects

```css
@keyframes miniplayer-gradient {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}
```

### Hover Animations

```css
/* Info panel reveal */
.show-on-hover {
  /* States:
       - Hidden
       - Revealing
       - Visible
       - Hiding
    */

  /* Transitions */
  transition: max-height 0.3s ease, filter 0.3s ease, opacity 0.3s ease, transform
      0.3s ease;
}
```

## Custom Controls

### Progress Bar Customization

```css
#zen-media-progress-bar {
  /* Track */
  &::-moz-range-track {
    height: var(--progress-height);
    transition: height 0.2s ease;
  }

  /* Thumb (playhead) */
  &::-moz-range-thumb {
    scale: 1;
    transition: scale 0.2s ease;
  }

  /* Progress fill */
  &::-moz-range-progress {
    height: var(--progress-height);
    transition: height 0.2s ease;
  }
}
```

### Time Display

```css
#zen-media-current-time,
#zen-media-duration {
  font-variant-numeric: tabular-nums;
}
```

## State Management

### Playback States

```css
/* Playing state */
#zen-media-controls-toolbar.playing {
  /* Gradient animation active */
}

/* Hover states */
#zen-media-controls-toolbar:hover {
  /* Show additional controls */
  /* Activate animations */
}
```

### Audio Indicator States

```css
/* Sound playing */
.tab-icon-overlay[soundplaying] {
  mask-image: var(--play-svg);
}

/* Muted */
.tab-icon-overlay[muted] {
  mask-image: var(--mute-svg);
}

/* Blocked */
.tab-icon-overlay[activemedia-blocked] {
  /* Custom blocked icon */
}
```

## Implementation Guidelines

### 1. State Transitions

- Implement smooth transitions between states
- Handle hover states appropriately
- Manage playback state changes
- Consider animation timing

### 2. Animation Performance

- Use hardware-accelerated properties
- Optimize gradient animations
- Handle hover animations efficiently
- Consider reduced motion preferences

### 3. Media Info Display

- Implement proper text truncation
- Handle long titles and artist names
- Manage info panel transitions
- Consider responsive layout

### 4. Audio Indicators

- Maintain indicator visibility
- Handle icon transitions
- Manage indicator states
- Consider tab state changes

## CSS Variables

```css
/* Core Variables */
--progress-height        /* Progress bar height */
--play-svg              /* Play icon SVG */
--mute-svg             /* Mute icon SVG */
--zen-primary-color   /* Primary theme color */
```

## Best Practices

1. **State Management**

   - Handle all media states
   - Implement proper transitions
   - Consider edge cases
   - Maintain state consistency

2. **Animation Performance**

   - Use efficient properties
   - Optimize transitions
   - Handle hover effects
   - Consider resource usage

3. **Audio Indicators**

   - Clear state indication
   - Smooth transitions
   - Proper icon scaling
   - Consistent positioning

4. **Accessibility**
   - Clear control labeling
   - Proper contrast
   - Keyboard navigation
   - Screen reader support

## Common Patterns

### Hover Effects

```css
/* Standard hover pattern */
element:hover {
  opacity: 0.8;
  transform: scale(1.11);
  transition: opacity 0.3s, transform 0.17s;
}
```

### Gradient Animations

```css
/* Gradient background */
element::before {
  content: "";
  background: radial-gradient(...);
  animation: miniplayer-gradient 10s infinite;
}
```

### State Transitions

```css
/* State change pattern */
element {
  transition: all 0.3s ease;

  &.active {
    /* Active state */
  }
}
```
