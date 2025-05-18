# Zen Browser Tab Organization Documentation

This document details Zen Browser's tab organization system, including groups, folders, and their associated features. This documentation is designed for AI models to understand and modify tab-related functionality.

## Tab Groups

### Group Container

```css
tab-group:not([split-view-group]) {
  /* Core group properties */
  display: flex;
  flex-direction: column;
  border-radius: var(--nebula-border-radius);
  transition: max-height 0.75s cubic-bezier(0.4, 0, 0.2, 1);

  /* States */
  &[collapsed]/* Collapsed state */
    &: not([collapsed]); /* Expanded state */
}
```

### Group Label

```css
.tab-group-label-container {
  /* Label container properties */
  position: sticky;
  top: 0;
  z-index: 1000;
  height: 36px;

  /* Label text */
  label {
    width: 100%;
    text-align: center;
    color: var(--sidebar-text-color);
    font-size: 12px;
  }
}
```

### Group States

```css
/* Collapsed state */
tab-group[collapsed] {
  max-height: 36px;

  /* Tab handling */
  tab {
    transform: translateY(-30px);
    opacity: 0;
  }
}

/* Expanded state */
tab-group:not([collapsed]) {
  max-height: 500px;
  overflow: scroll;
  scrollbar-width: none;
}
```

## Tabs Within Groups

### Tab Styling

```css
tab:not([zen-glance-tab]) {
  margin-left: 18px;
  overflow: visible;

  /* Click animation */
  &:active {
    transition: scale 0.03s;
    scale: 0.98;
  }
}
```

### Tab Indicators

```css
.tab-group-line {
  --tab-group-color-invert: transparent;
  --tab-group-color: transparent;
}

/* Range indicators */
@media (-moz-bool-pref: "tab.groups.display-tab-range") {
  tab::before {
    content: "";
    position: absolute;
    background-color: var(--tab-group-color);
    border-radius: 360px;
  }
}
```

## Group Icons and Controls

### Folder Icons

```css
.all-tabs-group-button .toolbarbutton-icon {
  width: 16px;
  height: 16px;
  mask-size: contain;

  /* Icon variants */
  @media (-moz-bool-pref: "tab.groups.fill-folders") {
    /* Filled folder style */
  }
  @media not (-moz-bool-pref: "tab.groups.fill-folders") {
    /* Outline folder style */
  }
}
```

### Arrow Indicators

```css
@media (-moz-bool-pref: "tab.groups.add-arrow") {
  .tab-group-label-container > label::after {
    content: "";
    width: 13px;
    height: 13px;
    transition: transform 0.5s cubic-bezier(0.25, 0.1, 0.25, 1);
  }
}
```

## Configuration Options

### Group Appearance

```css
/* Background options */
@media (-moz-bool-pref: "tab.groups.background") {
  background: var(--tab-group-color-invert);
}

/* Border options */
@media (-moz-bool-pref: "tab.groups.borders") {
  border-top: 1px solid #f2f2f21f;
  border-bottom: 1px solid #4442441f;
}
```

### Folder Theming

```css
@media (-moz-bool-pref: "tab.groups.theme-folders") {
  /* Themed folder icons */
}

@media (-moz-bool-pref: "tab.groups.fill-folders") {
  /* Filled folder icons */
}
```

## Context Menu Integration

### Group Context Menu

```css
#context_moveTabToGroupPopupMenu {
  .menu-iconic-icon {
    /* Icon styling */
    width: 16px;
    height: 16px;
    mask-size: contain;
  }
}
```

### Saved Groups

```css
.all-tabs-group-saved-group {
  clip: none;

  .toolbarbutton-icon {
    /* Icon styling */
    fill: var(--tab-group-color);
    width: 16px;
    height: 16px;
  }
}
```

## Implementation Guidelines

### 1. Group Management

- Handle collapse/expand states
- Manage tab transitions
- Maintain group hierarchy
- Handle group indicators

### 2. Visual Feedback

- Provide clear state indicators
- Implement smooth animations
- Handle icon transitions
- Maintain visual hierarchy

### 3. Performance

- Optimize scroll performance
- Handle tab animations
- Manage group transitions
- Consider memory usage

### 4. Accessibility

- Clear group indicators
- Readable labels
- Keyboard navigation
- Screen reader support

## Best Practices

### 1. State Management

```css
/* State transition pattern */
tab-group {
  transition: max-height 0.75s cubic-bezier(0.4, 0, 0.2, 1);

  &[collapsed] {
    /* Collapsed state */
  }

  &:not([collapsed]) {
    /* Expanded state */
  }
}
```

### 2. Animation Performance

```css
/* Efficient animations */
tab {
  transform-origin: left center;
  transition: transform 0.3s ease-out;
}
```

### 3. Visual Hierarchy

```css
/* Clear visual structure */
.tab-group-label-container {
  z-index: 1000;
  position: sticky;
  top: 0;
}
```

### 4. Error Prevention

```css
/* Handle edge cases */
tab-group:has(tab[hidden]) {
  display: none;
}
```

## Common Patterns

### 1. Group Transitions

```css
/* Smooth transitions */
transition: max-height 0.75s cubic-bezier(0.4, 0, 0.2, 1), opacity 0.3s ease;
```

### 2. Tab Animations

```css
/* Tab movement */
transform: translateY(-30px);
transition: transform 0.3s ease-out;
```

### 3. Icon Handling

```css
/* Icon styling */
mask-size: contain;
mask-repeat: no-repeat;
background-size: contain;
```

### 4. State Indicators

```css
/* Visual feedback */
background-color: var(--tab-group-color);
transition: background-color 0.2s ease;
```
