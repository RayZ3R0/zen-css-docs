# Zen Browser Picture-in-Picture (PiP) Documentation

This document details Zen Browser's Picture-in-Picture implementation, including selectors, states, and customization options. This is designed for AI models to understand and modify PiP functionality.

## Core Components

### Player Container

```css
.player-holder {
  /* Main video container */
  border-radius: 7.6px;
  clip-path: polygon(0 0, 100% 0, 100% 100%, 0 100%);
  /* Supports animation effects */
}
```

### Control Elements

```css
#controls {
  /* Main controls container */
  /* States:
       - showing
       - muted
       - playing
    */
}

#controls-bottom {
  /* Bottom control bar */
  /* Contains:
       - Scrubber
       - Buttons
       - Timestamps
    */
}

#settings {
  /* Settings panel */
  /* Contains:
       - Options
       - Sliders
       - Switches
    */
}
```

## Control Buttons

### Main Controls

```css
/* Core control buttons */
#playpause      /* Play/Pause toggle */
/* Play/Pause toggle */
#audio          /* Audio toggle */
#seekBackward   /* Seek -5s */
#seekForward    /* Seek +5s */
#fullscreen     /* Fullscreen toggle */
#close          /* Close PiP */
#minimize       /* Minimize PiP */
#unpip; /* Exit PiP mode */
```

### Button States

```css
button {
    /* States */
    &[disabled]     /* Disabled state */
    &:hover         /* Hover state */
    &:active        /* Active state */

    /* Common properties */
    transition: background-color 0.2s ease;
    border-radius: 6px;
}
```

## Media Controls

### Progress Bar

```css
#scrubber,
#audio-scrubber {
  &::-moz-range-track {
    /* Track styling */
  }

  &::-moz-range-progress {
    /* Progress fill */
  }

  &::-moz-range-thumb {
    /* Playhead */
    width: 12px;
    height: 12px;
  }
}
```

### Control Layout

```css
.controls-bottom-upper {
  /* Upper controls section */
  /* Contains scrubber */
}

.controls-bottom-lower {
  /* Lower controls section */
  /* Contains buttons */
}

.start-controls {
  /* Left-side controls */
  display: flex;
}

.end-controls {
  /* Right-side controls */
  display: flex;
}
```

## Animation System

### Appearance Animations

```css
@keyframes pip-fadein-html {
  0% {
    background-color: transparent;
    scale: 0.8;
  }
  100% {
    background-color: black;
    scale: 1;
  }
}

@keyframes pip-fadein-player {
  0% {
    filter: blur(30px);
    opacity: 0;
  }
  100% {
    filter: blur(0);
    opacity: 1;
  }
}
```

### Transition Effects

```css
/* Control transitions */
transition: opacity 0.3s ease, top 0.3s ease, filter 0.3s ease, background-color
    0.2s ease;

/* Panel transitions */
transition: backdrop-filter 0.3s ease, filter 0.3s ease;
```

## Theme Integration

### Variables

```css
:root {
    /* Colors */
    --nebula-primary-color: #a0d490;
    --nebula-colors-primary
    --nebula-colors-secondary

    /* Materials */
    --nebula-mat-hz-background
    --nebula-mat-hz-background-tinted
    --nebula-mat-hz-blur-radius

    /* Controls */
    --toolbarbutton-hover-background
    --toolbarbutton-active-background
}
```

### Material Effects

```css
/* Haze effect */
backdrop-filter: saturate() contrast() blur();
background-color: var(--nebula-mat-hz-background);

/* Glass effect */
background: var(--nebula-mat-ga-background);
box-shadow: var(--nebula-mat-ga-shadow-color);
```

## State Management

### Player States

```css
/* Fullscreen state */
body:fullscreen {
  /* Cursor management */
  &:has(#controls:not([showing])) {
    cursor: none;
  }
}

/* Control visibility */
#controls[showing]:hover {
  /* Show controls */
}

/* Playback states */
#controls.playing {
  /* Playing state */
}

#controls.muted {
  /* Muted state */
}
```

### Settings Panel

```css
#settings {
  /* States */
  &.hide {
    opacity: 0;
    pointer-events: none;
  }

  /* Panel layout */
  .panel-fieldset {
    backdrop-filter: blur(15px);
    transition: backdrop-filter 0.3s ease;
  }
}
```

## Implementation Guidelines

### 1. Animation Performance

- Use hardware-accelerated properties
- Implement smooth transitions
- Handle state changes efficiently
- Consider filter performance

### 2. State Management

- Handle all player states
- Manage control visibility
- Handle fullscreen transitions
- Maintain state consistency

### 3. Theme Integration

- Support light/dark modes
- Use CSS variables
- Implement material effects
- Handle blur effects

### 4. Control Layout

- Maintain proper spacing
- Handle responsive layout
- Manage control visibility
- Consider interaction areas

## Configuration Options

### PiP Preferences

```css
@media -moz-pref("nebula.pip.rounded") {
  /* Rounded corners support */
}

@media -moz-pref("nebula.pip.tinted-haze") {
  /* Tinted background effect */
}

@media -moz-pref("nebula.pip.native-border-radius") {
  /* Native radius support */
}
```

### Platform-Specific Handling

```css
@media (-moz-platform: windows) {
  /* Windows-specific styling */
}

@media (-moz-platform: macos) {
  /* macOS-specific styling */
}
```

## Best Practices

1. **Performance**

   - Optimize animations
   - Minimize repaints
   - Use efficient selectors
   - Handle transitions smoothly

2. **State Handling**

   - Clear state indicators
   - Smooth transitions
   - Proper cleanup
   - Error prevention

3. **User Experience**

   - Responsive controls
   - Clear feedback
   - Smooth animations
   - Proper scaling

4. **Theme Support**
   - Color scheme support
   - Material effects
   - Consistent styling
   - Platform adaptation
