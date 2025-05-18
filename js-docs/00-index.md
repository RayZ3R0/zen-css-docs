# Creating Firefox UserChrome.js Modifications

This documentation provides a comprehensive guide for AI models to create and maintain Firefox browser modifications using UserChrome.js (uc.js) files. It covers core concepts, implementation patterns, and real-world examples.

## Documentation Structure

1. [Core Concepts](01-core-concepts.md)

   - UserScript metadata structure
   - Module patterns and organization
   - Version handling and compatibility
   - File structure and naming conventions

2. [Browser Integration](02-browser-integration.md)

   - DOM access and manipulation
   - Browser API usage
   - Event handling and messaging
   - Security considerations

3. [UI Modifications](03-ui-modifications.md)

   - CSS injection patterns
   - XUL element creation
   - Theme integration
   - Animation handling

4. [State Management](04-state-management.md)

   - Configuration handling
   - Data persistence
   - Error handling
   - Cleanup procedures

5. [Examples](05-examples.md)
   - Basic modifications
   - UI enhancements
   - Feature extensions
   - Complex systems

## Key Concepts

UserChrome.js modifications allow deep customization of Firefox's interface and functionality. Key aspects to understand:

- Modifications run with chrome privileges
- Can access and modify browser internals
- Must handle version compatibility
- Should follow security best practices

## Common Patterns

Throughout the documentation, you'll see these recurring patterns:

1. Browser Integration:

   ```javascript
   // Access browser APIs
   Services.wm.getMostRecentWindow("navigator:browser");

   // Handle events
   gBrowser.tabContainer.addEventListener();
   ```

2. UI Modification:

   ```javascript
   // Create UI elements
   let element = document.createXULElement("button");

   // Inject styles
   let style = document.createElement("style");
   document.head.appendChild(style);
   ```

3. State Management:

   ```javascript
   // Handle configuration
   Services.prefs.getBoolPref("custom.preference");

   // Clean up on unload
   window.addEventListener("unload", cleanup);
   ```

## Getting Started

1. Start with [Core Concepts](01-core-concepts.md) to understand the fundamentals
2. Review [Browser Integration](02-browser-integration.md) for API usage
3. Explore [UI Modifications](03-ui-modifications.md) for interface changes
4. Learn about [State Management](04-state-management.md) for robust code
5. Study [Examples](05-examples.md) for practical implementations

Each section builds on the previous ones, providing a comprehensive understanding of uc.js development.

## Best Practices

1. Always include proper metadata in your scripts
2. Handle version compatibility explicitly
3. Clean up resources and event listeners
4. Use appropriate security checks
5. Follow Firefox's coding standards

## Additional Resources

- [MDN Web Docs](https://developer.mozilla.org/)
- [Firefox Source Docs](https://firefox-source-docs.mozilla.org/)
- [XUL Documentation](https://udn.realityripple.com/docs/Archive/Mozilla/XUL)
