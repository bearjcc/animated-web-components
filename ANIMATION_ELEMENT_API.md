# AnimationElement API Documentation

The `AnimationElement` class is the base class for all animated web components in this library. It extends `HTMLElement` and provides core animation functionality that other animation components inherit.

---

## Class Definition

```js
export default class AnimationElement extends HTMLElement
```

---

## Static Properties

### `#instances`
- **Type:** `Array<AnimationElement>`
- **Description:** Private static array that lists all instances of this class.
- **Usage:** Used internally to track all animation elements for batch processing.

### `#intervals`
- **Type:** `Object<number, number>`
- **Description:** Private static object containing all animation intervals.
- **Usage:** Helps keep animations in sync and increases performance by sharing intervals across elements with the same animation speed.

---

## Instance Properties

### `animationSpeed`
- **Type:** `number` (getter)
- **Description:** Number of milliseconds for each animation cycle.
- **Default:** 500ms (0.5 seconds)
- **Usage:**
  ```js
  // Get the current animation speed
  const speed = element.animationSpeed;
  ```
- **HTML Attribute:** `animation-duration` (in seconds, converted to milliseconds)

### `isAnimationEnabled`
- **Type:** `boolean`
- **Description:** Determines whether the animation should be running.
- **Default:** `false`
- **Usage:**
  ```js
  // Check if animation is enabled
  if (element.isAnimationEnabled) {
    console.log('Animation is running');
  }
  ```

---

## Constructor

### `constructor()`
- **Description:** Initializes a new AnimationElement instance.
- **Behavior:**
  - Calls `super()` to initialize the HTMLElement.
  - Adds the instance to the static `#instances` array.
  - Sets `isAnimationEnabled` based on the `animation-enabled` attribute.
  - Automatically enables animation if `animation-enabled` is set to `true`.
- **Usage:** Called automatically when creating new animation elements.

---

## Instance Methods

### `enableAnimation()`
- **Description:** Enables the animation for this element.
- **Behavior:**
  - Sets `isAnimationEnabled` to `true`.
  - Creates or reuses an interval for the animation speed.
  - Uses `requestAnimationFrame` for smooth animations.
  - Groups elements by animation speed for performance optimization.
- **Returns:** `void`
- **Usage:**
  ```js
  // Enable animation programmatically
  element.enableAnimation();
  ```

### `disableAnimation()`
- **Description:** Disables the animation for this element.
- **Behavior:**
  - Sets `isAnimationEnabled` to `false`.
  - The element will stop animating but remain in the instances array.
- **Returns:** `void`
- **Usage:**
  ```js
  // Disable animation programmatically
  element.disableAnimation();
  ```

### `animate()`
- **Description:** Abstract method that should be implemented by subclasses.
- **Behavior:** This method is called by the animation loop for each enabled element.
- **Returns:** `void` (implementation dependent)
- **Usage:** Override in subclasses to define specific animation behavior.

---

## HTML Attributes

### `animation-enabled`
- **Type:** `boolean`
- **Description:** Enables or disables the animation on initialization.
- **Values:** `"true"` or `"false"`
- **Default:** `false`
- **Usage:**
  ```html
  <my-animation-element animation-enabled="true">
    <!-- content -->
  </my-animation-element>
  ```

### `animation-duration`
- **Type:** `number`
- **Description:** Sets the animation speed in seconds.
- **Default:** `0.5` (500ms)
- **Usage:**
  ```html
  <my-animation-element animation-duration="1.0">
    <!-- content -->
  </my-animation-element>
  ```

---

## Usage Examples

### Basic Usage
```html
<my-animation-element animation-enabled="true" animation-duration="1.0">
  <div>Animated content</div>
</my-animation-element>
```

### Programmatic Control
```js
// Get an animation element
const element = document.querySelector('my-animation-element');

// Check if animation is enabled
console.log(element.isAnimationEnabled);

// Get animation speed
console.log(element.animationSpeed);

// Enable/disable animation
element.enableAnimation();
element.disableAnimation();
```

### Creating a Custom Animation Element
```js
import AnimationElement from './animation-element.js';

class MyCustomAnimation extends AnimationElement {
  constructor() {
    super();
    // Custom initialization
  }

  animate() {
    // Implement your animation logic here
    // This method is called by the animation loop
    console.log('Animating...');
  }
}

customElements.define('my-custom-animation', MyCustomAnimation);
```

---

## Performance Considerations

1. **Shared Intervals:** Elements with the same animation speed share intervals for better performance.
2. **requestAnimationFrame:** Uses `requestAnimationFrame` for smooth animations.
3. **Batch Processing:** All enabled elements with the same speed are processed together.
4. **Memory Management:** Elements remain in the instances array even when disabled.

---

## Inheritance Pattern

The `AnimationElement` class is designed to be extended by specific animation components:

- `TypingElement` – Creates typewriter effects
- `FloatingRandomElement` – Creates floating animations
- `BouncingBall` – Creates bouncing ball animations
- `ProgressBarElement` – Creates animated progress bars

Each subclass should:
1. Call `super()` in the constructor
2. Implement the `animate()` method
3. Optionally override other methods as needed

---

## Browser Compatibility

This class uses modern web standards:
- Custom Elements (Web Components)
- Shadow DOM
- `requestAnimationFrame`
- ES6+ features (classes, private fields, etc.)

Ensure your target browsers support these features or use appropriate polyfills. 