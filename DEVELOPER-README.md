# CurationsLA Golden Boy 4.0 — Developer Guide

## Overview

The **Golden Boy 4.0** system is CurationsLA's programmatic design architecture for generating consistent editorial illustrations, section dividers, and content graphics. This system combines retro LA poster aesthetics with modern editorial brutalism.

This guide provides developers with everything needed to implement, render, and maintain Golden Boy-compliant visuals across all platforms.

---

## Table of Contents

1. [Quick Start](#quick-start)
2. [System Architecture](#system-architecture)
3. [Installation](#installation)
4. [Core Specifications](#core-specifications)
5. [Implementation Guide](#implementation-guide)
6. [API Reference](#api-reference)
7. [Examples](#examples)
8. [Testing & Validation](#testing--validation)
9. [Troubleshooting](#troubleshooting)
10. [Contributing](#contributing)

---

## Quick Start

### Minimal Example

```javascript
// Import the Golden Boy renderer
import { GoldenBoyRenderer } from './goldenboy-renderer';

// Create a new divider
const divider = new GoldenBoyRenderer({
  sectionTitle: 'EATS',
  primaryIcon: 'taco',
  secondaryIcons: ['knife', 'fork'],
  barColor: '#ff79cb', // Persian Pink
  canvas: { width: 3400, height: 1000 }
});

// Render to SVG
const svg = divider.render('svg');

// Or render to Canvas
const canvas = divider.render('canvas');
```

---

## System Architecture

### Design Philosophy

**"Digital Brutalism reimagined through Los Angeles retro poster culture."**

The Golden Boy system is grounded in:
- Honest shapes
- Hard edges
- Flat inks
- High-contrast outline logic
- Function-driven layouts
- Cinematic LA sensibility

### Core Principles

1. **Ultra-Structured Layout**: 3.4:1 aspect ratio with rigid 60/40 split
2. **Outline-First Graphics**: Everything uses 4px black outlines
3. **Flat Color Fields**: No gradients, no soft shadows, no photorealism
4. **Geometric Silhouettes**: Chunky, blocky, angular shapes
5. **Editorial Typography**: Heavy geometric sans-serif with white fill and black outline
6. **Retro Print Aesthetic**: Subtle grain texture, screenprint feel

---

## Installation

### NPM Package (Coming Soon)

```bash
npm install @curationsla/golden-boy
```

### Manual Setup

1. Clone this repository
2. Copy `golden-boy-schema.json` and `figma-tokens.json` to your project
3. Implement the renderer using the specifications below

---

## Core Specifications

### Canvas

| Property | Value | Type | Required |
|----------|-------|------|----------|
| Aspect Ratio | 3.4:1 | String | Yes |
| Width | 3400px | Number | Recommended |
| Height | 1000px | Number | Recommended |

### Frame

| Property | Value | Type | Required |
|----------|-------|------|----------|
| Stroke Width | 4px | Number | Yes |
| Stroke Color | #000000 | Color | Yes |
| Corner Radius | 0px | Number | Yes |
| Stroke Join | miter | String | Yes |
| Stroke Cap | butt | String | Yes |

### Shadow

| Property | Value | Type | Required |
|----------|-------|------|----------|
| Offset X | 24px | Number | Yes |
| Offset Y | 24px | Number | Yes |
| Color | #000000 | Color | Yes |
| Blur | 0px | Number | Yes (hard edge) |

### Left Bar

| Property | Value | Type | Required |
|----------|-------|------|----------|
| Width | 60% | Percentage | Yes |
| Fill | Palette color | Color | Yes |
| Texture | retro_grain | String | Yes |
| Texture Opacity | 2-4% | Number | Yes |

### Typography

| Property | Value | Type | Required |
|----------|-------|------|----------|
| Font Family | Heavy Geometric Sans | String | Yes |
| Font Weight | 900 | Number | Yes |
| Case | UPPERCASE | String | Yes |
| Fill | #FFFFFF | Color | Yes |
| Stroke | #000000 | Color | Yes |
| Stroke Width | 4px | Number | Yes |
| Letter Spacing | -2% to -4% | Percentage | Yes |
| Alignment | center vertical, left horizontal | String | Yes |

### Icon Cluster

| Property | Value | Type | Required |
|----------|-------|------|----------|
| Zone Width | 40% | Percentage | Yes |
| Icon Count | 3 (1 primary, 2 secondary) | Number | Yes |
| Outline Width | 4px | Number | Yes |
| Primary Pop | 18% above frame | Percentage | Yes |
| Secondary Size | 60-75% of primary | Percentage | Yes |
| Style | Geometric, blocky, chunky | String | Yes |

### Color Palette

| Name | Hex | Usage |
|------|-----|-------|
| Mindaro | #ebf99a | Energetic, vibrant sections |
| Persian Pink | #ff79cb | Creative, cultural sections |
| Medium Slate Blue | #8b5cf6 | Tech, innovation sections |
| Vivid Sky Blue | #4cc9f0 | Outdoor, lifestyle sections |
| Seasalt | #f8f8f8 | Backgrounds, subtle fills |
| Black | #000000 | Outlines, frames, shadows |
| White | #FFFFFF | Text fills |

---

## Implementation Guide

### SVG Renderer

```javascript
class GoldenBoySVGRenderer {
  constructor(config) {
    this.config = this.validateConfig(config);
  }

  validateConfig(config) {
    // Validate against golden-boy-schema.json
    // Ensure all required properties are present
    // Throw error if any prohibitions are violated
    return config;
  }

  render() {
    const svg = this.createSVGElement();

    // Layer 1: Shadow block
    this.renderShadow(svg);

    // Layer 2: Frame
    this.renderFrame(svg);

    // Layer 3: Left bar with grain
    this.renderLeftBar(svg);

    // Layer 4: Typography
    this.renderTypography(svg);

    // Layer 5: Icon cluster
    this.renderIcons(svg);

    return svg;
  }

  renderShadow(svg) {
    const shadow = document.createElementNS('http://www.w3.org/2000/svg', 'rect');
    shadow.setAttribute('x', '24');
    shadow.setAttribute('y', '24');
    shadow.setAttribute('width', this.config.canvas.width);
    shadow.setAttribute('height', this.config.canvas.height);
    shadow.setAttribute('fill', '#000000');
    svg.appendChild(shadow);
  }

  renderFrame(svg) {
    const frame = document.createElementNS('http://www.w3.org/2000/svg', 'rect');
    frame.setAttribute('x', '0');
    frame.setAttribute('y', '0');
    frame.setAttribute('width', this.config.canvas.width);
    frame.setAttribute('height', this.config.canvas.height);
    frame.setAttribute('fill', 'none');
    frame.setAttribute('stroke', '#000000');
    frame.setAttribute('stroke-width', '4');
    frame.setAttribute('stroke-linejoin', 'miter');
    frame.setAttribute('stroke-linecap', 'butt');
    svg.appendChild(frame);
  }

  renderLeftBar(svg) {
    const barWidth = this.config.canvas.width * 0.6;
    const bar = document.createElementNS('http://www.w3.org/2000/svg', 'rect');
    bar.setAttribute('x', '0');
    bar.setAttribute('y', '0');
    bar.setAttribute('width', barWidth);
    bar.setAttribute('height', this.config.canvas.height);
    bar.setAttribute('fill', this.config.leftBar.color);

    // Apply grain texture
    this.applyGrainTexture(svg, bar);

    svg.appendChild(bar);
  }

  applyGrainTexture(svg, element) {
    // Create SVG filter for grain texture
    const filter = document.createElementNS('http://www.w3.org/2000/svg', 'filter');
    filter.setAttribute('id', 'grain');

    const turbulence = document.createElementNS('http://www.w3.org/2000/svg', 'feTurbulence');
    turbulence.setAttribute('type', 'fractalNoise');
    turbulence.setAttribute('baseFrequency', '0.9');
    turbulence.setAttribute('numOctaves', '4');
    turbulence.setAttribute('result', 'noise');

    const blend = document.createElementNS('http://www.w3.org/2000/svg', 'feBlend');
    blend.setAttribute('in', 'SourceGraphic');
    blend.setAttribute('in2', 'noise');
    blend.setAttribute('mode', 'multiply');
    blend.setAttribute('result', 'blend');

    const opacity = document.createElementNS('http://www.w3.org/2000/svg', 'feComponentTransfer');
    const funcA = document.createElementNS('http://www.w3.org/2000/svg', 'feFuncA');
    funcA.setAttribute('type', 'linear');
    funcA.setAttribute('slope', '0.03'); // 3% opacity
    opacity.appendChild(funcA);

    filter.appendChild(turbulence);
    filter.appendChild(blend);
    filter.appendChild(opacity);

    svg.appendChild(filter);
    element.setAttribute('filter', 'url(#grain)');
  }

  renderTypography(svg) {
    const text = document.createElementNS('http://www.w3.org/2000/svg', 'text');
    const barWidth = this.config.canvas.width * 0.6;

    text.setAttribute('x', '100');
    text.setAttribute('y', this.config.canvas.height / 2);
    text.setAttribute('dominant-baseline', 'middle');
    text.setAttribute('text-anchor', 'start');
    text.setAttribute('font-family', 'Futura Extra Bold, Gotham Black, sans-serif');
    text.setAttribute('font-weight', '900');
    text.setAttribute('font-size', '180');
    text.setAttribute('letter-spacing', '-0.03em');
    text.setAttribute('fill', '#FFFFFF');
    text.setAttribute('stroke', '#000000');
    text.setAttribute('stroke-width', '4');
    text.setAttribute('paint-order', 'stroke fill');
    text.textContent = this.config.sectionTitle.toUpperCase();

    svg.appendChild(text);
  }

  renderIcons(svg) {
    const iconZoneX = this.config.canvas.width * 0.6;
    const iconZoneWidth = this.config.canvas.width * 0.4;

    // Primary icon (pops 18% above frame)
    this.renderPrimaryIcon(svg, iconZoneX, iconZoneWidth);

    // Secondary icons
    this.renderSecondaryIcons(svg, iconZoneX, iconZoneWidth);
  }

  renderPrimaryIcon(svg, x, width) {
    // Icon rendering logic based on icon type
    // This would be customized based on the specific icon needed
    const iconGroup = document.createElementNS('http://www.w3.org/2000/svg', 'g');
    iconGroup.setAttribute('transform', `translate(${x + width/2}, ${this.config.canvas.height * 0.5})`);

    // Example: Simple geometric shape
    const shape = document.createElementNS('http://www.w3.org/2000/svg', 'rect');
    shape.setAttribute('x', '-100');
    shape.setAttribute('y', '-150'); // Extended above frame
    shape.setAttribute('width', '200');
    shape.setAttribute('height', '300');
    shape.setAttribute('fill', this.config.primaryIcon.fill);
    shape.setAttribute('stroke', '#000000');
    shape.setAttribute('stroke-width', '4');

    iconGroup.appendChild(shape);
    svg.appendChild(iconGroup);
  }

  renderSecondaryIcons(svg, x, width) {
    // Secondary icon rendering logic
    // Positioned to flank or orbit the primary icon
  }

  createSVGElement() {
    const svg = document.createElementNS('http://www.w3.org/2000/svg', 'svg');
    svg.setAttribute('width', this.config.canvas.width);
    svg.setAttribute('height', this.config.canvas.height);
    svg.setAttribute('viewBox', `0 0 ${this.config.canvas.width} ${this.config.canvas.height}`);
    svg.setAttribute('xmlns', 'http://www.w3.org/2000/svg');
    return svg;
  }
}
```

### Canvas Renderer

```javascript
class GoldenBoyCanvasRenderer {
  constructor(config) {
    this.config = this.validateConfig(config);
    this.canvas = document.createElement('canvas');
    this.canvas.width = config.canvas.width;
    this.canvas.height = config.canvas.height;
    this.ctx = this.canvas.getContext('2d');
  }

  render() {
    // Layer 1: Shadow
    this.renderShadow();

    // Layer 2: Left bar with grain
    this.renderLeftBar();

    // Layer 3: Frame
    this.renderFrame();

    // Layer 4: Typography
    this.renderTypography();

    // Layer 5: Icons
    this.renderIcons();

    return this.canvas;
  }

  renderShadow() {
    this.ctx.fillStyle = '#000000';
    this.ctx.fillRect(24, 24, this.config.canvas.width, this.config.canvas.height);
  }

  renderFrame() {
    this.ctx.strokeStyle = '#000000';
    this.ctx.lineWidth = 4;
    this.ctx.lineJoin = 'miter';
    this.ctx.lineCap = 'butt';
    this.ctx.strokeRect(2, 2, this.config.canvas.width - 4, this.config.canvas.height - 4);
  }

  renderLeftBar() {
    const barWidth = this.config.canvas.width * 0.6;
    this.ctx.fillStyle = this.config.leftBar.color;
    this.ctx.fillRect(0, 0, barWidth, this.config.canvas.height);

    // Apply grain texture
    this.applyGrainTexture(0, 0, barWidth, this.config.canvas.height);
  }

  applyGrainTexture(x, y, width, height) {
    const imageData = this.ctx.getImageData(x, y, width, height);
    const data = imageData.data;

    for (let i = 0; i < data.length; i += 4) {
      const noise = (Math.random() - 0.5) * 10;
      data[i] += noise;     // R
      data[i + 1] += noise; // G
      data[i + 2] += noise; // B
    }

    this.ctx.putImageData(imageData, x, y);
  }

  renderTypography() {
    this.ctx.font = '900 180px "Futura Extra Bold", "Gotham Black", sans-serif';
    this.ctx.textAlign = 'left';
    this.ctx.textBaseline = 'middle';

    const text = this.config.sectionTitle.toUpperCase();
    const x = 100;
    const y = this.config.canvas.height / 2;

    // Stroke
    this.ctx.strokeStyle = '#000000';
    this.ctx.lineWidth = 4;
    this.ctx.lineJoin = 'miter';
    this.ctx.strokeText(text, x, y);

    // Fill
    this.ctx.fillStyle = '#FFFFFF';
    this.ctx.fillText(text, x, y);
  }

  renderIcons() {
    // Icon rendering logic
  }

  validateConfig(config) {
    // Validation logic
    return config;
  }
}
```

---

## API Reference

### `GoldenBoyRenderer`

Main renderer class for creating Golden Boy 4.0 compliant graphics.

#### Constructor

```javascript
new GoldenBoyRenderer(config)
```

**Parameters:**

- `config` (Object): Configuration object
  - `sectionTitle` (String): Section name (e.g., "EATS", "EVENTS")
  - `primaryIcon` (Object): Primary icon configuration
    - `type` (String): Icon type identifier
    - `fill` (String): Fill color from palette
  - `secondaryIcons` (Array): Array of secondary icon objects
  - `leftBar` (Object): Left bar configuration
    - `color` (String): Bar color from palette
  - `canvas` (Object): Canvas dimensions
    - `width` (Number): Canvas width
    - `height` (Number): Canvas height

#### Methods

##### `render(format)`

Renders the Golden Boy graphic in the specified format.

**Parameters:**
- `format` (String): Output format ('svg', 'canvas', 'png', 'json')

**Returns:**
- SVG element, Canvas element, PNG data URL, or JSON specification

##### `validate()`

Validates the current configuration against Golden Boy 4.0 schema.

**Returns:**
- `true` if valid, throws error if invalid

##### `export(format, filename)`

Exports the rendered graphic to a file.

**Parameters:**
- `format` (String): Export format ('svg', 'png', 'pdf')
- `filename` (String): Output filename

---

## Examples

### Example 1: EATS Section Divider

```javascript
const eatsDivider = new GoldenBoyRenderer({
  sectionTitle: 'EATS',
  leftBar: {
    color: '#ff79cb' // Persian Pink
  },
  primaryIcon: {
    type: 'taco',
    fill: '#ebf99a' // Mindaro
  },
  secondaryIcons: [
    { type: 'knife', fill: '#4cc9f0' },
    { type: 'fork', fill: '#8b5cf6' }
  ],
  canvas: {
    width: 3400,
    height: 1000
  }
});

const svg = eatsDivider.render('svg');
document.body.appendChild(svg);
```

### Example 2: EVENTS Section Divider

```javascript
const eventsDivider = new GoldenBoyRenderer({
  sectionTitle: 'EVENTS',
  leftBar: {
    color: '#8b5cf6' // Medium Slate Blue
  },
  primaryIcon: {
    type: 'calendar',
    fill: '#ff79cb' // Persian Pink
  },
  secondaryIcons: [
    { type: 'ticket', fill: '#ebf99a' },
    { type: 'star', fill: '#4cc9f0' }
  ],
  canvas: {
    width: 3400,
    height: 1000
  }
});

const canvas = eventsDivider.render('canvas');
```

### Example 3: Custom Icon Set

```javascript
const customDivider = new GoldenBoyRenderer({
  sectionTitle: 'COMMUNITY',
  leftBar: {
    color: '#4cc9f0' // Vivid Sky Blue
  },
  primaryIcon: {
    type: 'custom',
    paths: [
      // SVG path data for custom icon
      'M 100,100 L 200,100 L 200,200 L 100,200 Z'
    ],
    fill: '#ebf99a'
  },
  secondaryIcons: [
    {
      type: 'custom',
      paths: ['M 50,50 L 100,50 L 100,100 L 50,100 Z'],
      fill: '#ff79cb'
    },
    {
      type: 'custom',
      paths: ['M 150,150 L 200,150 L 200,200 L 150,200 Z'],
      fill: '#8b5cf6'
    }
  ],
  canvas: {
    width: 3400,
    height: 1000
  }
});
```

---

## Testing & Validation

### Schema Validation

Use the provided JSON schema to validate configurations:

```javascript
import Ajv from 'ajv';
import schema from './golden-boy-schema.json';

const ajv = new Ajv();
const validate = ajv.compile(schema);

function validateGoldenBoyConfig(config) {
  const valid = validate(config);
  if (!valid) {
    console.error('Validation errors:', validate.errors);
    throw new Error('Invalid Golden Boy configuration');
  }
  return true;
}
```

### Visual Regression Testing

```javascript
import { expect } from 'chai';
import { GoldenBoyRenderer } from './goldenboy-renderer';
import { compareImages } from 'image-comparison-tool';

describe('Golden Boy Renderer', () => {
  it('should render EATS divider correctly', async () => {
    const renderer = new GoldenBoyRenderer(eatsConfig);
    const output = renderer.render('png');
    const baseline = './test/baselines/eats-divider.png';

    const diff = await compareImages(output, baseline);
    expect(diff).to.be.below(0.01); // Less than 1% difference
  });
});
```

### Prohibition Checks

```javascript
function checkProhibitions(config) {
  const prohibitions = {
    gradients: /gradient/i,
    roundedCorners: config.frame.cornerRadius > 0,
    softShadows: config.shadow.blur > 0,
    wrongOutlineWidth: config.iconCluster.outlineWidth !== 4
  };

  const violations = [];

  if (prohibitions.roundedCorners) {
    violations.push('Rounded corners detected (must be 0)');
  }

  if (prohibitions.softShadows) {
    violations.push('Soft shadow detected (blur must be 0)');
  }

  if (prohibitions.wrongOutlineWidth) {
    violations.push('Incorrect outline width (must be 4px)');
  }

  if (violations.length > 0) {
    throw new Error(`Prohibition violations: ${violations.join(', ')}`);
  }

  return true;
}
```

---

## Troubleshooting

### Common Issues

#### Issue: Icons not popping above frame

**Solution:** Ensure the primary icon Y position extends 18% above the frame boundary.

```javascript
const popAmount = config.canvas.height * 0.18;
iconY = 0 - popAmount; // Negative Y to extend above
```

#### Issue: Grain texture too strong

**Solution:** Reduce opacity to 2-4% range.

```javascript
grainOpacity = 0.03; // 3%
```

#### Issue: Text outline not visible

**Solution:** Ensure `paint-order: stroke fill` for SVG, or draw stroke before fill for Canvas.

```javascript
// SVG
text.setAttribute('paint-order', 'stroke fill');

// Canvas
ctx.strokeText(text, x, y);
ctx.fillText(text, x, y);
```

#### Issue: Colors don't match palette

**Solution:** Always use exact hex values from the approved palette.

```javascript
const PALETTE = {
  mindaro: '#ebf99a',
  persianPink: '#ff79cb',
  mediumSlateBlue: '#8b5cf6',
  vividSkyBlue: '#4cc9f0',
  seasalt: '#f8f8f8',
  black: '#000000'
};
```

---

## Contributing

### Guidelines

1. All contributions must maintain Golden Boy 4.0 architectural integrity
2. No exceptions to prohibitions (gradients, rounded corners, etc.)
3. Test all changes against the JSON schema
4. Include visual regression tests
5. Document any new icon types or variations

### Pull Request Process

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/new-icon-set`)
3. Validate against schema (`npm run validate`)
4. Run tests (`npm test`)
5. Submit pull request with screenshots

### Adding New Icons

```javascript
// 1. Create icon definition
const newIcon = {
  type: 'palm-tree',
  category: 'lifestyle',
  paths: [
    // SVG path data
  ],
  defaultFill: '#4cc9f0',
  outlineWidth: 4
};

// 2. Validate geometric style
validateIconStyle(newIcon);

// 3. Add to icon library
iconLibrary.register(newIcon);
```

---

## License

© 2025 CurationsLA. All rights reserved.

---

## Support

For questions, issues, or feature requests:
- GitHub Issues: [github.com/curationsla/golden-boy/issues](https://github.com/curationsla/golden-boy/issues)
- Email: dev@curationsla.com
- Documentation: [docs.curationsla.com/golden-boy](https://docs.curationsla.com/golden-boy)

---

## Changelog

### Version 4.0.0 (Current)
- Initial release of Golden Boy 4.0 system
- Complete architectural overhaul
- JSON schema and Figma token support
- Multi-renderer support (SVG, Canvas, PNG)
- Comprehensive developer documentation

---

**Remember:** The Golden Boy system is non-negotiable. Every element must follow the architectural rules precisely to maintain visual consistency and brand integrity across all CurationsLA platforms.
