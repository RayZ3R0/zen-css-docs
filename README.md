# Zen Browser Technical Documentation

This is a comprehensive technical documentation of Zen Browser's CSS system, specifically designed for AI consumption and modification. The documentation provides detailed information about selectors, states, animations, and features.

## Core Documentation

### 1. [Selectors](./zen-browser-selectors.md)

Detailed documentation of CSS selectors and their relationships:

- Selector hierarchy
- Component structure
- DOM relationships
- Specificity patterns

### 2. [State Management](./zen-browser-states.md)

Analysis of browser states and feature management:

- Core states
- State transitions
- Feature integration
- Error prevention

### 3. [Animations](./zen-browser-animations.md)

Complete guide to animation capabilities:

- Animation primitives
- Transition systems
- Performance optimization
- Hardware acceleration

## Feature Documentation

### 4. [Media Controls](./zen-browser-media.md)

Documentation of media playback features:

- Media control components
- Audio states
- Progress indicators
- Volume controls

### 5. [Picture-in-Picture](./zen-browser-pip.md)

Details of PiP implementation:

- PiP controls
- Window management
- Transition effects
- Platform specifics

### 6. [Tab Organization](./zen-browser-tabs.md)

Tab management system documentation:

- Tab groups
- Folder structure
- Visual indicators
- State transitions

### 7. [Workspace Management](./zen-browser-workspaces.md)

Workspace system documentation:

- Workspace controls
- Indicator system
- Style variations
- State handling

## Component Architecture

### Core Layout

```
zen-main-app-wrapper/
├── browser/
│   ├── browserStack/
│   └── tabbrowser-tabpanels/
├── urlbar/
│   ├── urlbar-background
│   └── urlbar-input-container/
└── zen-tabs-wrapper/
    └── tabbrowser-tab/
```

### State Hierarchy

```
Root States
├── Theme States [data-theme]
├── Layout States [zen-*]
└── Feature States
    ├── Component States
    └── Interaction States
```

## Implementation Guidelines

### 1. State Management

- Use appropriate attribute selectors
- Maintain state consistency
- Handle transitions properly
- Implement cleanup

### 2. Animation Performance

- Use hardware-accelerated properties
- Optimize transitions
- Handle hover animations
- Consider reduced motion

### 3. Feature Integration

- Follow established patterns
- Maintain compatibility
- Consider dependencies
- Test interactions

### 4. Error Prevention

- Validate states
- Handle edge cases
- Provide fallbacks
- Clean up resources

## CSS Variable Reference

### Theme Variables

```css
--zen-primary-color        /* Primary theme color */
--zen-branding-bg-reverse  /* Reverse branding color */
--zen-colors-tertiary     /* Tertiary theme color */
```

### Layout Variables

```css
--zen-element-separation  /* Element spacing */
--nebula-border-radius   /* UI corner radius */
--sidebar-width          /* Sidebar width */
```

### Component Variables

```css
--toolbarbutton-hover-background
--progress-height
--tab-min-width
```

## Common Patterns

### 1. State Transitions

```css
/* Standard transition pattern */
transition: opacity 0.3s ease, transform 0.2s ease;
```

### 2. Hardware Acceleration

```css
/* Performance optimization */
transform: translate3d(0, 0, 0);
backface-visibility: hidden;
will-change: transform;
```

### 3. Theme Support

```css
/* Theme-aware styling */
color: light-dark(black, white);
background: var(--zen-colors-tertiary);
```

### 4. Error Handling

```css
/* Fallback pattern */
@supports not (backdrop-filter: blur(10px)) {
  /* Fallback styles */
}
```

## Best Practices Summary

1. **Selector Usage**

   - Use specific selectors
   - Maintain hierarchy
   - Follow naming conventions
   - Consider specificity

2. **State Management**

   - Handle all states
   - Manage transitions
   - Prevent conflicts
   - Clean up properly

3. **Performance**

   - Optimize animations
   - Efficient selectors
   - Manage resources
   - Monitor impact

4. **Integration**
   - Theme compatibility
   - State compatibility
   - Feature compatibility
   - Layout compatibility

## Testing Requirements

### 1. Visual Testing

- Theme compatibility
- Animation smoothness
- State transitions
- Layout integrity

### 2. Performance Testing

- Animation performance
- Transition smoothness
- Resource usage
- Layout performance

### 3. State Testing

- State transitions
- State combinations
- Error conditions
- Edge cases

### 4. Integration Testing

- Feature interactions
- Theme switching
- Layout modes
- Platform compatibility

## Resources

### Documentation Files

- [Selectors](./zen-browser-selectors.md)
- [States](./zen-browser-states.md)
- [Animations](./zen-browser-animations.md)
- [Media](./zen-browser-media.md)
- [PiP](./zen-browser-pip.md)
- [Tabs](./zen-browser-tabs.md)
- [Workspaces](./zen-browser-workspaces.md)

This documentation is designed to be machine-readable and provides precise technical details for AI models to effectively modify and extend Zen Browser's UI while maintaining consistency and performance.
