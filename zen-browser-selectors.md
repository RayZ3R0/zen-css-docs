# Zen Browser Core Selectors Documentation

## Browser Components

### Main Structure

```css
/* Core layout elements */
#zen-main-app-wrapper
  #browser
  .browserStack
  #tabbrowser-tabpanels
  .browserSidebarContainer;
```

### Tab System

```css
/* Tab container */
#tabbrowser-tabs {
  /* Holds all browser tabs */
}

/* Individual tab states */
.tabbrowser-tab[selected]      /* Selected tab */
.tabbrowser-tab[busy]         /* Loading tab */
.tabbrowser-tab[pending]      /* Pending load */
.tabbrowser-tab[pinned]; /* Pinned tab */
```

### URL Bar

```css
#urlbar {
    /* States */
    &[open]          /* Open state */
    &[focused]       /* Focus state */
}

#urlbar-input-container
#urlbar-background
#identity-icon-box
```

## Special Features

### PDF Viewer

```css
/* PDF viewer structure */
#viewerContainer #toolbarContainer #loadingBar #sidebarContainer #toolbarViewer;
```

### Settings Pages

```css
/* Settings variables */
:root {
    --zen-colors-tertiary
    --zen-settings-secondary-background
    --zen-colors-border-contrast
}

/* Settings elements */
groupbox
.zen-settings-panel
```

### Ctrl+Tab Preview

```css
#ctrlTab-panel
  .ctrlTab-preview
  .ctrlTab-preview-inner
  .ctrlTab-canvas
  .ctrlTab-favicon-container;
```

## State Management

### Browser States

```css
[zen-split="true"]           /* Split view active */
[zen-glance="true"]         /* Glance view active */
[zen-compact-mode="true"]   /* Compact mode active */
```

### Tab States

```css
.deck-selected              /* Selected panel */
/* Selected panel */
[visuallyselected="true"]  /* Visually selected */
[zen-glance-tab]; /* Glance tab state */
```

### Animation States

```css
.zen-glance-background     /* Glance animation active */
/* Glance animation active */
[zen-split]; /* Split view transition */
```

## UI Components

### Toolbar Elements

```css
#zen-appcontent-wrapper #zen-appcontent-navbar-container #zen-tabbox-wrapper;
```

### Sidebar Components

```css
#sidebar-box #zen-sidebar-web-panel .sidebar-panel;
```

### Workspace Controls

```css
#zen-workspaces-button #zen-current-workspace-indicator-container;
```

## CSS Variables

### Theme Variables

```css
--zen-colors-tertiary
--zen-settings-secondary-background
--zen-primary-color
--zen-branding-bg-reverse
```

### Layout Variables

```css
--zen-element-separation
--sidebar-width
```

## Interactive Features

### Trackpad Gestures

```css
#historySwipeAnimationPreviousArrow #historySwipeAnimationNextArrow;
```

### Split View

```css
.browserSidebarContainer[zen-split="true"]
  #tabbrowser-tabpanels[zen-split="true"];
```

## Best Practices

1. State Management

   - Use zen-\* attributes for core states
   - Respect existing transition patterns
   - Handle all possible state combinations

2. Animation Integration

   - Follow established animation timing functions
   - Use appropriate transition properties
   - Consider performance implications

3. Layout Modifications

   - Maintain proper element spacing
   - Respect the split view system
   - Consider compact mode compatibility

4. Theme Compatibility
   - Support both light and dark themes
   - Use CSS variables for colors
   - Handle transparency properly

## Common Patterns

### State Transitions

```css
/* Standard transition pattern */
transition: all 0.3s cubic-bezier(0.175, 0.885, 0.32, 1.35);

/* Quick transition */
transition: opacity 0.2s ease-out;
```

### Layout Patterns

```css
/* Centered content */
position: absolute;
top: 50%;
left: 50%;
transform: translate(-50%, -50%);

/* Full container */
position: absolute;
inset: 0;
```

### Animation Patterns

```css
/* Smooth scale */
transform: scale(0.95);
transition: transform 0.2s ease;

/* Fade effect */
opacity: 0;
transition: opacity 0.3s ease;
```

## Implementation Guidelines

1. **Selector Usage**

   - Use specific selectors for targeting elements
   - Follow the established hierarchy
   - Consider selector performance

2. **State Management**

   - Handle all possible states
   - Implement proper transitions
   - Clean up states appropriately

3. **Performance**

   - Use efficient selectors
   - Optimize animations
   - Consider resource usage

4. **Compatibility**
   - Support all browser modes
   - Handle theme variations
   - Consider accessibility
