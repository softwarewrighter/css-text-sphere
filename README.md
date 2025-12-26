# CSS Text Sphere

A pure HTML/CSS 3D text orbiting sphere demo. No JavaScript, Rust, or frameworks required - just modern CSS3 transforms and animations.

## Features

- 3D sphere rendered with pure CSS gradients and shadows
- 17 individual letters orbiting in 3D space using CSS transforms
- Text moves in front of and behind the sphere using Z-axis depth
- Rainbow-colored letters with matching glow effects
- Smooth continuous clockwise rotation animation
- Fully responsive and performant (60 FPS)
- Zero JavaScript - pure static HTML/CSS files

## Live Demo

To view locally:

```bash
# Install basic-http-server if needed
cargo install basic-http-server

# Serve the docs directory
basic-http-server -a 0.0.0.0:8080 docs
```

Then open `http://localhost:8080` in your browser.

## Tech Stack

- **HTML5** - Semantic structure
- **CSS3** - 3D transforms, animations, gradients, shadows
- **CSS Keyframes** - Smooth rotation animations
- **CSS Variables** - Color theming (future)
- **Static Files** - No build process, no JavaScript

## Project Structure

```
css-text-sphere/
+-- docs/
|   +-- index.html          # Main HTML file
|   +-- styles.css          # All CSS styles and animations
+-- documentation/          # Project documentation
|   +-- architecture.md     # System design
|   +-- design.md           # Technical implementation
|   +-- plan.md             # Implementation phases
|   +-- prd.md              # Product requirements
|   +-- status.md           # Current progress
+-- images/                # Screenshots and assets
+-- README.md              # This file
```

## Browser Support

- Chrome 57+
- Firefox 52+
- Safari 11+
- Edge 16+

All modern browsers support CSS3 3D transforms and keyframe animations.

## Documentation

- [Product Requirements](documentation/prd.md) - Goals and requirements
- [Architecture](documentation/architecture.md) - System design
- [Design](documentation/design.md) - Technical implementation details
- [Plan](documentation/plan.md) - Implementation phases
- [Status](documentation/status.md) - Current progress

## Related Projects

This is part of a series exploring text sphere animations with different technologies:

- [wasm-text-sphere](https://github.com/softwarewrighter/wasm-text-sphere) - Rust/WASM + WebGL
- [three-text-sphere](https://github.com/softwarewrighter/three-text-sphere) - Three.js
- [d3-text-sphere](https://github.com/softwarewrighter/d3-text-sphere) - D3.js
- [godot-text-sphere](https://github.com/softwarewrighter/godot-text-sphere) - Godot Engine

## License

MIT
