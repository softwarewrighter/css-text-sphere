# Architecture

## System Overview

The CSS Text Sphere is a pure frontend application consisting of two static files:

1. `docs/index.html` - HTML structure
2. `docs/styles.css` - All styles and animations

There is no backend, no build process, and no JavaScript runtime.

## Component Architecture

### Scene Layer (`.scene`)

- Acts as the 3D viewport container
- Sets `perspective: 1000px` for 3D depth perception
- Provides the coordinate system for all 3D transforms

### Sphere Layer (`.sphere`)

- Centered 3D sphere rendered with CSS gradients
- Position: absolute, centered in scene
- Transform: translate(-50%, -50%) translateZ(0px) - center the element
- Style: radial-gradient for 3D lighting effect
- Box-shadows: inset shadows for depth, outer glow for atmosphere

### Orbit Container Layer (`.orbit-container`)

- Wraps all text elements
- Position: absolute, full size of scene
- Transform-style: preserve-3d - enables 3D positioning for children

### Text Layers (`.text` + `.orbit-N`)

- 17 individual letter elements
- Each positioned at the same absolute location initially
- Each has a unique animation delay for staggered positioning
- Transform-style: preserve-3d - enables 3D transforms

## Animation System

### Keyframes Animation (`@keyframes orbit`)

The animation uses a sequence of transforms to create 3D rotation:

1. **Start (0%)**: rotateY(180deg) translateZ(160px) rotateY(-180deg)
   - Text is behind the sphere (negative Z)
   - Opacity: 0 (invisible)

2. **Fade In (10-20%)**: Gradual opacity increase to 1
   - Text moves toward the front

3. **Front Center (50%)**: rotateY(0deg) translateZ(160px) rotateY(0deg)
   - Text is directly in front of sphere (positive Z)
   - Opacity: 1 (fully visible)

4. **Fade Out (80-90%)**: Gradual opacity decrease to 0
   - Text moves toward the back

5. **End (100%)**: rotateY(-180deg) translateZ(160px) rotateY(180deg)
   - Text is behind the sphere again
   - Opacity: 0 (invisible)

### Timing and Delays

- Total animation duration: 6 seconds per full orbit
- 17 letters evenly spaced: 6s / 17 = ~0.353s between each letter
- First letter starts at 0s, subsequent letters at 0.353s intervals
- Last letter starts at 5.647s (16 * 0.353s)

### 3D Transform Mathematics

Each text element uses a three-part transform chain:

```
rotateY(angle) -> translateZ(distance) -> rotateY(-angle)
```

1. `rotateY(angle)`: Positions the element at a specific angle around the Y-axis
2. `translateZ(160px)`: Moves the element outward from the center by 160px (orbit radius)
3. `rotateY(-angle)`: Counter-rotates the element so text always faces the viewer

This technique ensures letters always face forward while orbiting in 3D space.

## Color System

Each letter is assigned a unique color from a predefined palette:

- `.orbit-1`: #ff6b6b (red)
- `.orbit-2`: #feca57 (yellow)
- `.orbit-3`: #48dbfb (cyan)
- `.orbit-4`: #ff9ff3 (pink)
- `.orbit-5`: #54a0ff (blue)
- `.orbit-6`: #5f27cd (purple)
- Pattern repeats every 6 colors

The `text-shadow` uses `currentColor` to match each letter's base color.

## Data Flow

There is no runtime data flow. The entire animation is pre-defined in CSS:

1. Browser loads HTML -> creates DOM tree
2. Browser loads CSS -> applies styles and animations
3. Browser renders frames at 60 FPS using CSS animation engine
4. No JavaScript execution needed

## Browser Rendering Pipeline

1. **Parse HTML**: Create DOM tree
2. **Parse CSS**: Create CSSOM tree
3. **Combine**: Create render tree
4. **Layout**: Calculate element positions (includes 3D transforms)
5. **Paint**: Draw each element to layers
6. **Composite**: Combine layers with 3D depth sorting
7. **Display**: Present final frame

CSS 3D transforms utilize GPU acceleration for smooth 60 FPS performance.

## Performance Characteristics

### Optimizations

- **GPU Acceleration**: CSS transforms are hardware-accelerated
- **Composite Layers**: Each text element is a separate layer
- **No JavaScript**: Zero JS execution overhead
- **No Layout Triggers**: Transforms don't cause reflows
- **Minimal Repaint**: Only changed properties (opacity, transform) repaint

### Metrics

- **Initial Load**: < 1ms (no JavaScript to parse/execute)
- **Frame Time**: ~16ms (60 FPS)
- **Memory**: < 1 MB (no runtime objects)
- **CPU Usage**: Minimal (CSS animation handled by browser engine)
- **GPU Usage**: Low to moderate (layer compositing)

## Browser Compatibility Matrix

| Feature | Chrome 57+ | Firefox 52+ | Safari 11+ | Edge 16+ |
|---------|-----------|-------------|------------|----------|
| CSS 3D Transforms | Yes | Yes | Yes | Yes |
| CSS Keyframes | Yes | Yes | Yes | Yes |
| transform-style: preserve-3d | Yes | Yes | Yes | Yes |
| perspective | Yes | Yes | Yes | Yes |
| text-shadow | Yes | Yes | Yes | Yes |
| radial-gradient | Yes | Yes | Yes | Yes |

## Deployment Architecture

### Static File Serving

Any HTTP server can serve the static files:

```
basic-http-server -a 0.0.0.0:8080 docs
```

The application works on:

- GitHub Pages
- Netlify
- Vercel
- AWS S3 (static website hosting)
- Any static file server

### File Structure

```
/
+-- docs/
    +-- index.html
    +-- styles.css
```

No build step, no bundling, no minification required.
