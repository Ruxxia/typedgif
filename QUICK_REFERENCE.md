# Typing Animation GIF Generator - Quick Reference

## 1-Minute Setup

```bash
# Create new Nuxt project
npx nuxi@latest init my-typing-gif
cd my-typing-gif

# Copy TypingAnimationGif.vue to components/

# Edit pages/index.vue
# Add this:
<template>
  <TypingAnimationGif />
</template>

# Run
npm run dev
# Visit http://localhost:3000
```

## Key Properties & How to Change Them

### Animation Speed
```javascript
const speed = ref(80)  // milliseconds per character
// Lower = faster animation = smaller GIF
```

### Font Size
```javascript
const fontSize = ref(48)  // pixels
// Affects text readability and rendering time
```

### Canvas Dimensions
```html
<canvas 
  ref="canvas" 
  width="800"     // Change here
  height="300"    // And here
></canvas>
```

### Frame Rate
```javascript
const frameRate = 30  // fps
// 30 = smooth, 60 = smoother but larger file
```

### Text Position
In `drawFrame()` function:
```javascript
ctx.fillText(displayText, 50, 150)
//                        x   y
// Change x=50 to move left/right
// Change y=150 to move up/down
```

### Cursor Position
```javascript
cursorX = 50 + ctx.measureText(displayText).width
// Cursor draws at start position (50) + text width
```

### GIF Quality
```javascript
const gif = new GIF({
  quality: 10  // 1-30, lower = better compression
})
```

## Common Customizations

### 1. Change Default Text
```javascript
const text = ref('Your text here')
```

### 2. Add Drop Shadow
```javascript
// In drawFrame(), before drawing text:
ctx.shadowColor = 'rgba(0, 0, 0, 0.5)'
ctx.shadowOffsetX = 3
ctx.shadowOffsetY = 3
ctx.shadowBlur = 5
ctx.fillText(displayText, 50, 150)
ctx.shadowColor = 'transparent'
```

### 3. Use Custom Font
```javascript
ctx.font = `${fontSize.value}px 'Courier New'`
// or use a Google Font
```

### 4. Add Outline Text
```javascript
ctx.strokeStyle = textColor.value
ctx.lineWidth = 2
ctx.strokeText(displayText, 50, 150)
ctx.fillStyle = bgColor.value
ctx.fillText(displayText, 50, 150)
```

### 5. Change Text Alignment
```javascript
ctx.textAlign = 'center'  // 'start', 'center', 'end'
ctx.textBaseline = 'middle'  // 'top', 'middle', 'bottom'
```

### 6. Add Watermark
```javascript
ctx.font = '12px Arial'
ctx.fillStyle = 'rgba(0,0,0,0.3)'
ctx.textAlign = 'right'
ctx.fillText('© 2024', canvas.value.width - 10, canvas.value.height - 10)
```

### 7. Multi-line Text
```javascript
function drawFrame(frameIndex) {
  const lines = text.value.split('\n')
  let y = 100
  
  lines.forEach((line, idx) => {
    const charCount = Math.floor(progress * line.length)
    ctx.fillText(line.substring(0, charCount), 50, y + (idx * 40))
  })
}
```

### 8. Gradient Text
```javascript
const gradient = ctx.createLinearGradient(0, 0, canvas.width, 0)
gradient.addColorStop(0, '#ff0000')
gradient.addColorStop(1, '#00ff00')
ctx.fillStyle = gradient
ctx.fillText(displayText, 50, 150)
```

### 9. Animated Background Color
```javascript
const hue = (frameIndex / totalFrames) * 360
ctx.fillStyle = `hsl(${hue}, 100%, 50%)`
ctx.fillRect(0, 0, canvas.width, canvas.height)
```

### 10. Add Background Image
```javascript
const bgImg = new Image()
bgImg.src = '/background.png'
bgImg.onload = () => {
  ctx.drawImage(bgImg, 0, 0, canvas.width, canvas.height)
}
```

## Performance Optimization

### Reduce GIF Size
```javascript
// Option 1: Faster animation
const speed = ref(40)  // Instead of 80

// Option 2: Lower quality
const gif = new GIF({ quality: 5 })  // Instead of 10

// Option 3: Smaller canvas
width="600" height="250"  // Instead of 800x300

// Option 4: Shorter text
// Fewer characters = fewer frames
```

### Faster GIF Generation
```javascript
// Reduce workers
const gif = new GIF({
  workers: 1  // Instead of 2
})

// Reduce frame rate
const frameRate = 20  // Instead of 30
```

## Debugging Tips

### Check Frame Generation
```javascript
// Log each frame
frames.forEach((frame, idx) => {
  if (idx % 10 === 0) console.log(`Frame ${idx} captured`)
})
```

### Test Canvas Drawing
```javascript
// Draw test patterns
ctx.fillStyle = '#ff0000'
ctx.fillRect(0, 0, 100, 100)
```

### Monitor File Size
```javascript
gif.on('finished', function(blob) {
  console.log('GIF size:', (blob.size / 1024).toFixed(1), 'KB')
})
```

### Check Browser Console
- Press F12 in browser
- Look for red error messages
- Check Network tab to verify gif.js loads

## Animation Timing Formula

```
Total Frames = (Text Length × Speed / 1000) × Frame Rate

Example:
Text: "Hello World" (11 chars)
Speed: 80ms per char
Frame Rate: 30 fps

Total = (11 × 80 / 1000) × 30
Total = 0.88 × 30
Total = 26.4 frames ≈ 26 frames
Duration = 26 / 30 ≈ 0.87 seconds
```

## File Size Estimation

```
Rough formula:
GIF Size ≈ (Width × Height × 4 × Total Frames) / (1024² × Quality Factor)

Example:
800 × 300 × 4 × 30 frames / (1024² × 10)
≈ 28.8 MB / 10.5 = ~2.7 MB

Real-world: Usually 30-40% smaller due to dithering
```

## API Methods (if converted to composable)

```javascript
// Start animation preview
preview()

// Generate and download GIF
generateGif()

// Update statistics
updateStats()

// Load gif.js library
loadGifLib()
```

## CSS Customization

### Change button colors
```css
.generate-btn {
  background: #your-color;
}

.generate-btn:hover {
  background: #darker-color;
}
```

### Change input styling
```css
.form-group input {
  border-radius: 12px;
  border: 2px solid #your-color;
}
```

### Adjust layout
```css
.form-row {
  grid-template-columns: 1fr 2fr;  /* Instead of 1fr 1fr */
}
```

## Common Errors & Fixes

### "gif is not defined"
```
Fix: gif.js library didn't load. Check internet connection.
```

### "Canvas width/height should be positive"
```
Fix: Ensure width and height are numbers and > 0
```

### "Cannot read property 'getContext' of null"
```
Fix: Canvas not mounted. Check ref="canvas" is correct.
```

### "Uncaught SyntaxError: Unexpected token"
```
Fix: Missing closing bracket or quote. Check code syntax.
```

## Useful Links

- [Canvas API Docs](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)
- [gif.js Library](https://github.com/jnordberg/gif.js)
- [Nuxt 3 Docs](https://nuxt.com)
- [Vue 3 Docs](https://vuejs.org)

## Feature Comparison: Animation Types

| Type | Effect | Best For | Speed |
|------|--------|----------|-------|
| **Char** | Typewriter | Code, tutorials | Fast |
| **Word** | Word slide | Titles, headlines | Medium |
| **Fade** | Fade in | Artistic, subtle | Slow |

## Quick Feature Checklist

- [x] Real-time preview
- [x] Multiple animation types
- [x] Customizable colors
- [x] Adjustable speed & size
- [x] Cursor animation
- [x] Progress tracking
- [x] One-click download
- [ ] Multiple text colors (could add)
- [ ] Background video (could add)
- [ ] Keyboard shortcuts (could add)

---

**Pro tip**: Start with default settings, generate a GIF, then tweak one property at a time to see what works best for your use case.
