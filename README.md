# Keyboard Accessibility Guide

[![WCAG 2.1 AA](https://img.shields.io/badge/WCAG-2.1_AA-5C6BC0.svg)](https://www.w3.org/TR/WCAG21/)

A comprehensive guide to implementing keyboard accessibility in web applications, ensuring your site is usable by everyone, regardless of their input method.

## Table of Contents

- [Why Keyboard Accessibility Matters](#why-keyboard-accessibility-matters)
- [Core Principles](#core-principles)
- [Common Keyboard Controls](#common-keyboard-controls)
- [Implementation Guide](#implementation-guide)
- [Testing Methodology](#testing-methodology)
- [Common Issues & Solutions](#common-issues--solutions)
- [Advanced Patterns](#advanced-patterns)
- [Resources](#resources)
- [Contributing](#contributing)
- [License](#license)

## Why Keyboard Accessibility Matters

Keyboard accessibility is a fundamental aspect of web accessibility that benefits various user groups:

- **Users with motor impairments** who cannot use a mouse
- **Screen reader users** who rely on keyboard navigation
- **Power users** who prefer keyboard shortcuts
- **Temporary situations** where mouse use is impractical
- **Legal compliance** with accessibility standards (WCAG, ADA, Section 508)

## Core Principles

1. **All interactive elements** must be reachable via keyboard
2. **Logical focus order** that follows the document flow
3. **Visible focus indicators** for all interactive elements
4. **Keyboard traps** must be avoided
5. **Consistent behavior** across all interactive elements

## Common Keyboard Controls

| Key Combination | Action |
|-----------------|--------|
| `Tab` | Move to next focusable element |
| `Shift + Tab` | Move to previous focusable element |
| `Enter` / `Space` | Activate buttons and links |
| `Arrow Keys` | Navigate within components (menus, sliders, etc.) |
| `Escape` | Close modals/dialogs |
| `Home` / `End` | Jump to start/end of content |
| `Page Up` / `Page Down` | Scroll content |

## Implementation Guide

### 1. Semantic HTML

Always prefer native HTML elements:

```html
<!-- Good -->
<button>Submit</button>
<a href="#">Learn more</a>

<!-- Avoid -->
<div onclick="submit()">Submit</div>
<span class="link">Learn more</span>
```

### 2. Custom Controls

For custom interactive elements, ensure proper ARIA roles and keyboard support:

```html
<div 
  role="button" 
  tabindex="0"
  class="custom-button"
  @click="handleClick"
  @keydown="handleKeyDown"
>
  Custom Button
</div>
```

```javascript
function handleKeyDown(event) {
  if (event.key === 'Enter' || event.key === ' ') {
    event.preventDefault();
    this.click();
  }
}
```

### 3. Managing Focus

- Use `element.focus()` to move focus programmatically
- Implement focus trapping for modals
- Return focus to the triggering element when closing dialogs
- Use `inert` attribute or `aria-hidden` for off-screen content

## Testing Methodology

### Manual Testing
1. Navigate using only `Tab`, `Shift+Tab`, `Enter`, and arrow keys
2. Verify all interactive elements are reachable
3. Check for logical focus order
4. Ensure focus indicators are visible

### Automated Testing Tools
- [Axe](https://www.deque.com/axe/)
- [Lighthouse](https://developers.google.com/web/tools/lighthouse)
- [WAVE](https://wave.webaim.org/)

## Common Issues & Solutions

| Issue | Solution |
|-------|----------|
| **Non-focusable elements** | Add `tabindex="0"` and proper ARIA roles |
| **Missing focus styles** | Add visible `:focus` styles |
| **Focus traps** | Ensure focus can escape modals |
| **Inconsistent behavior** | Match native element behavior |
| **Hidden focusable elements** | Use `display: none` or `visibility: hidden` |

## Advanced Patterns

### Modal Dialogs
```javascript
// Open modal
document.querySelector('.modal').showModal();

// Trap focus inside modal
const focusableElements = 'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])';
const modal = document.querySelector('.modal');
const focusableContent = modal.querySelectorAll(focusableElements);
const firstFocusableElement = focusableContent[0];
const lastFocusableElement = focusableContent[focusableContent.length - 1];

firstFocusableElement.focus();

modal.addEventListener('keydown', function(e) {
  if (e.key === 'Tab') {
    if (e.shiftKey) {
      if (document.activeElement === firstFocusableElement) {
        lastFocusableElement.focus();
        e.preventDefault();
      }
    } else {
      if (document.activeElement === lastFocusableElement) {
        firstFocusableElement.focus();
        e.preventDefault();
      }
    }
  }
  if (e.key === 'Escape') {
    modal.close();
  }
});
```

## Resources

- [WebAIM: Keyboard Accessibility](https://webaim.org/techniques/keyboard/)
- [W3C WAI-ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)
- [MDN: Keyboard Navigation](https://developer.mozilla.org/en-US/docs/Web/Accessibility/Keyboard-navigable_JavaScript_widgets)
- [A11Y Project Checklist](https://www.a11yproject.com/checklist/)

