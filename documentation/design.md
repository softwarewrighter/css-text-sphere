# Design

## Visual Design

### Color Palette

#### Background
- Linear gradient: #1a1a2e -> #16213e -> #0f3460
- Creates depth and modern dark theme
- High contrast for text readability

#### Sphere
- Radial gradient centered at 30% 30%
- Colors: #4a9eff (highlight) -> #0066cc (mid) -> #003366 (shadow) -> #001a33 (dark)
- Creates 3D sphere appearance with light source from upper left
- Inset box-shadows add internal depth
- Outer glow (0 0 60px) adds atmosphere

#### Letters (Rainbow Palette)
- #ff6b6b - Red
- #feca57 - Yellow
- #48dbfb - Cyan
- #ff9ff3 - Pink
- #54a0ff - Blue
- #5f27cd - Purple

Pattern repeats across 17 letters.

### Typography

- Font: 'Courier New', monospace
- Size: 48px
- Weight: Bold
- Monospace ensures consistent letter width and bracket alignment

### Visual Effects

#### Text Glow
- `text-shadow: 0 0 20px currentColor`
- Each letter's glow matches its base color
- Creates neon/billboard effect

#### Sphere Lighting
- Radial gradient simulates 3D sphere lighting
- Highlight at 30% 30% (upper-left)
- Shadows toward bottom-right
- Internal inset shadows enhance depth

## Animation Design

### Motion Path

Text follows a circular orbit around the sphere:

1. **Behind Sphere** (180deg): Start position, invisible
2. **Right Side** (135deg): Fade in begins
3. **Front Center** (0deg): Fully visible, closest to viewer
4. **Left Side** (-135deg): Fade out begins
5. **Behind Sphere** (-180deg): End position, invisible

### Timing Function

- Type: `linear` for consistent speed throughout orbit
- Duration: 6 seconds per full revolution
- Smooth, continuous looping

### Opacity Keyframes

- 0%: opacity: 0 (behind sphere)
- 10%: opacity: 0.3 (starting to fade in)
- 20%: opacity: 1 (fully visible)
- 50%: opacity: 1 (front center, fully visible)
- 80%: opacity: 0.3 (starting to fade out)
- 90%: opacity: 0 (behind sphere)
- 100%: opacity: 0 (end, behind sphere)

Smooth opacity transitions create natural fade in/out effect.

### Rotation Mathematics

The transform sequence creates the orbit effect:

```
1. rotateY(angle) - Position around Y-axis
2. translateZ(160px) - Push outward to orbit radius
3. rotateY(-angle) - Counter-rotate to face viewer
```

This ensures text always faces the camera while orbiting in 3D space.

### Staggered Delays

17 letters with evenly distributed delays:

```
delay = (index * 6s) / 17
```

Examples:
- Letter 1: 0s
- Letter 2: 0.353s
- Letter 9 (middle): 2.824s
- Letter 17: 5.647s

This creates continuous text flow around the sphere.

## Layout Design

### Scene Container

- Width: 400px
- Height: 400px
- Perspective: 1000px
- Centered in viewport using flexbox on body

### Sphere

- Width: 200px
- Height: 200px
- Centered in scene (left: 50%, top: 50%)
- Radius: 100px
- Positioned at Z = 0

### Text Elements

- Width: 100% of scene (400px)
- Height: 100% of scene (400px)
- Absolute positioning (top: 0, left: 0)
- Flex centered content (justify-center, align-center)
- Positioned at Z = 160px (orbit radius)

## CSS Architecture

### File Structure

Single `styles.css` file with logical sections:

1. Reset and Global Styles
2. Body and Layout
3. Scene Container
4. Sphere Styling
5. Orbit Container
6. Text Element Base Styles
7. Individual Text Orbits (17 classes with delays)
8. Color Classes (17 classes)
9. Keyframe Animation

### Naming Conventions

- BEM-like naming: `.scene`, `.sphere`, `.orbit-container`, `.text`
- Modifier classes: `.orbit-1`, `.orbit-2`, etc.
- Descriptive names: clear purpose and usage

### CSS Properties by Function

#### 3D Transforms
- `perspective`: Scene depth
- `transform-style: preserve-3d`: Enable 3D children
- `rotateY()`: Y-axis rotation
- `translateZ()`: Z-axis positioning

#### Animation
- `animation`: shorthand for animation properties
- `animation-name`: keyframes reference
- `animation-duration`: 6s
- `animation-timing-function`: linear
- `animation-delay`: staggered timing
- `animation-iteration-count`: infinite
- `animation-fill-mode: backwards`: pre-animation state

#### Visual Effects
- `background`: linear and radial gradients
- `box-shadow`: depth and glow effects
- `text-shadow`: neon text glow
- `opacity`: fade in/out
- `color`: letter colors

#### Layout
- `position`: absolute for layering
- `display`: flex for centering
- `justify-content`: horizontal alignment
- `align-items`: vertical alignment
- `left`/`top`: absolute positioning

## Responsive Design

### Current Implementation

Fixed size (400px scene, 200px sphere) centered in viewport.

### Future Enhancements

Potential responsive improvements:

1. Use viewport units (`vmin`, `vw`, `vh`) for scaling
2. Media queries for mobile sizing
3. Fluid font sizes using `clamp()`
4. Adjustable orbit radius for smaller screens

## Accessibility Considerations

### Current State

- Semantic HTML structure (divs are appropriate here)
- High contrast colors
- Readable text size
- Pure CSS (works with JS disabled)

### Potential Improvements

1. Add `aria-label` to scene container for screen readers
2. Provide alternative text description
3. Respect `prefers-reduced-motion` media query

## Browser Fallbacks

### No JavaScript Required

The application functions without JavaScript. No fallbacks needed.

### CSS Feature Support

### Feature Detection (Optional)

If needed, could add:

```css
@supports not (transform-style: preserve-3d) {
  .orbit-container {
    display: none;
  }
}
```

But given modern browser support, this is not necessary.

## Performance Optimization

### Current Optimizations

1. **Hardware Acceleration**: CSS transforms use GPU
2. **No Layout Thrashing**: Transforms don't cause reflows
3. **Minimal Paint**: Only opacity and transform change
4. **Static HTML**: No DOM manipulation overhead
5. **No JavaScript**: Zero JS parsing/execution time

### Further Optimizations

If needed:

1. Use `will-change: transform` for text elements
2. Reduce number of text elements if performance issues
3. Simplify box-shadows (combine into single declaration)
4. Use CSS containment

## Maintainability

### Code Organization

- Clear section comments in CSS
- Logical grouping of related properties
- Consistent indentation and formatting
- Descriptive class names

### Customization Points

Easy to modify:

1. Text content: Edit HTML divs
2. Colors: Change `.orbit-N` color values
3. Timing: Adjust animation duration and delays
4. Size: Modify `.scene` and `.sphere` dimensions
5. Orbit radius: Change `translateZ()` value
6. Letter count: Add/remove HTML divs and CSS classes
