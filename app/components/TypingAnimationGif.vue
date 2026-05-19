<template>
  <div class="typing-gif-generator">
    <div class="container">
      <h1>Typing Animation to GIF Converter (Enhanced)</h1>
      
      <!-- Input Section -->
      <div class="input-section">
        <!-- Main Text Input -->
        <div class="form-group">
          <label for="text-input">Text to animate (use \n for line breaks)</label>
          <textarea 
            id="text-input"
            v-model="text" 
            placeholder="Enter text... Use \n for new lines&#10;e.g: Hello\nWorld"
            rows="4"
          ></textarea>
          <small class="help-text">💡 Tip: Type \n in the text to create line breaks. Each line animates separately.</small>
        </div>

        <!-- Row 1: Animation Type & Speed -->
        <div class="form-row">
          <div class="form-group">
            <label for="animation-type">Animation type</label>
            <select id="animation-type" v-model="animationType">
              <option value="char">Character by character</option>
              <option value="word">Word slide-in</option>
              <option value="fade">Character fade-in</option>
            </select>
          </div>

          <div class="form-group">
            <label for="speed">Speed (ms per char)</label>
            <input 
              id="speed"
              v-model.number="speed" 
              type="range" 
              min="20" 
              max="200" 
              step="10"
            >
            <span class="value-display">{{ speed }}ms</span>
          </div>
        </div>

        <!-- Row 2: Font Size & Text Color -->
        <div class="form-row">
          <div class="form-group">
            <label for="font-size">Font size</label>
            <input 
              id="font-size"
              v-model.number="fontSize" 
              type="range" 
              min="16" 
              max="80" 
              step="4"
            >
            <span class="value-display">{{ fontSize }}px</span>
          </div>

          <div class="form-group">
            <label for="text-color">Text color</label>
            <input 
              id="text-color"
              v-model="textColor" 
              type="color"
            >
          </div>
        </div>

        <!-- Row 3: Background Color & Cursor -->
        <div class="form-row">
          <div class="form-group">
            <label for="bg-color">Background color</label>
            <input 
              id="bg-color"
              v-model="bgColor" 
              type="color"
            >
          </div>

          <div class="form-group">
            <label for="show-cursor">
              <input 
                id="show-cursor"
                v-model="showCursor" 
                type="checkbox"
              >
              Show cursor
            </label>
          </div>
        </div>

        <!-- TEXT EFFECTS SECTION -->
        <div class="effects-divider">
          <h3>Text Effects</h3>
        </div>

        <!-- Row 4: Shadow Effect -->
        <div class="form-row">
          <div class="form-group">
            <label for="shadow-toggle">
              <input 
                id="shadow-toggle"
                v-model="enableShadow" 
                type="checkbox"
              >
              Drop shadow
            </label>
          </div>

          <div class="form-group" v-if="enableShadow">
            <label for="shadow-color">Shadow color</label>
            <input 
              id="shadow-color"
              v-model="shadowColor" 
              type="color"
            >
          </div>
        </div>

        <!-- Row 5: Shadow Offset & Blur -->
        <div class="form-row" v-if="enableShadow">
          <div class="form-group">
            <label for="shadow-offset-x">Shadow offset X</label>
            <input 
              id="shadow-offset-x"
              v-model.number="shadowOffsetX" 
              type="range" 
              min="-10" 
              max="10" 
              step="1"
            >
            <span class="value-display">{{ shadowOffsetX }}px</span>
          </div>

          <div class="form-group">
            <label for="shadow-blur">Shadow blur</label>
            <input 
              id="shadow-blur"
              v-model.number="shadowBlur" 
              type="range" 
              min="0" 
              max="20" 
              step="1"
            >
            <span class="value-display">{{ shadowBlur }}px</span>
          </div>
        </div>

        <!-- Row 6: Outline Effect -->
        <div class="form-row">
          <div class="form-group">
            <label for="outline-toggle">
              <input 
                id="outline-toggle"
                v-model="enableOutline" 
                type="checkbox"
              >
              Text outline
            </label>
          </div>

          <div class="form-group" v-if="enableOutline">
            <label for="outline-color">Outline color</label>
            <input 
              id="outline-color"
              v-model="outlineColor" 
              type="color"
            >
          </div>
        </div>

        <!-- Row 7: Outline Width -->
        <div class="form-row" v-if="enableOutline">
          <div class="form-group">
            <label for="outline-width">Outline width</label>
            <input 
              id="outline-width"
              v-model.number="outlineWidth" 
              type="range" 
              min="1" 
              max="5" 
              step="0.5"
            >
            <span class="value-display">{{ outlineWidth }}px</span>
          </div>

          <div class="form-group"></div>
        </div>

        <!-- Row 8: Gradient Text -->
        <div class="form-row">
          <div class="form-group">
            <label for="gradient-toggle">
              <input 
                id="gradient-toggle"
                v-model="enableGradient" 
                type="checkbox"
              >
              Gradient text
            </label>
          </div>

          <div class="form-group" v-if="enableGradient">
            <label>Gradient colors</label>
            <div class="color-pair">
              <input 
                v-model="gradientColor1" 
                type="color"
                title="Start color"
              >
              <span>→</span>
              <input 
                v-model="gradientColor2" 
                type="color"
                title="End color"
              >
            </div>
          </div>
        </div>

        <!-- Preview & Generate Buttons -->
        <div class="button-group">
          <button 
            class="preview-btn" 
            @click="previewAnimation"
            :disabled="!text || isGenerating"
          >
            {{ isAnimating ? 'Playing...' : 'Preview' }}
          </button>

          <button 
            class="generate-btn" 
            @click="generateGif"
            :disabled="!text || isGenerating || isAnimating"
          >
            {{ isGenerating ? 'Generating GIF...' : 'Download GIF' }}
          </button>
        </div>
      </div>

      <!-- Canvas Preview -->
      <div class="canvas-section">
        <div class="canvas-wrapper">
          <canvas 
            ref="canvas" 
            :width="canvasWidth" 
            :height="canvasHeight"
            class="animation-canvas"
          ></canvas>
        </div>
      </div>

      <!-- Progress Bar -->
      <div v-if="isGenerating" class="progress">
        <div class="progress-bar" :style="{ width: progress + '%' }"></div>
        <span class="progress-text">{{ progress }}%</span>
      </div>

      <!-- Info Section -->
      <div class="info-section">
        <p><strong>Estimated GIF size:</strong> {{ estimatedSize }}</p>
        <p><strong>Animation duration:</strong> {{ animationDuration }}s</p>
        <p><strong>Total frames:</strong> {{ totalFrames }}</p>
        <p><strong>Lines:</strong> {{ lineCount }}</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue'

// Core animation properties
const text = ref('Hello\nWorld!')
const animationType = ref('char')
const speed = ref(80)
const fontSize = ref(48)
const textColor = ref('#000000')
const bgColor = ref('#ffffff')
const showCursor = ref(true)

const canvas = ref(null)
const maxLineWidth = ref(0)

// Text effects
const enableShadow = ref(true)
const shadowColor = ref('#000000')
const shadowOffsetX = ref(2)
const shadowBlur = ref(4)

const enableOutline = ref(false)
const outlineColor = ref('#333333')
const outlineWidth = ref(2)

const enableGradient = ref(false)
const gradientColor1 = ref('#ff0000')
const gradientColor2 = ref('#0000ff')

// State
const isGenerating = ref(false)
const isAnimating = ref(false)
const progress = ref(0)

const canvasWidth = ref(500)
const canvasHeight = ref(500)

// Computed properties
const lines = computed(() => text.value.split('\n'))
const lineCount = computed(() => lines.value.length)
const totalCharacters = computed(() => 
  lines.value.reduce((sum, line) => sum + line.length, 0)
)

const animationDuration = computed(() => {
  return (totalCharacters.value * speed.value / 1000).toFixed(2)
})

const frameRate = 30
const totalFrames = computed(() => {
  return Math.ceil((totalCharacters.value * speed.value / 1000) * frameRate)
})

const estimatedSize = computed(() => {
  const frameSize = (canvasWidth.value * canvasHeight.value * 4) / 1024
  const totalSize = frameSize * totalFrames.value
  return (totalSize / 1024).toFixed(1) + ' KB'
})

// Auto-size logic
function updateCanvasSize() {
  if (!import.meta.client || !text.value) return
  
  const tempCtx = document.createElement('canvas').getContext('2d')
  tempCtx.font = `${fontSize.value}px Arial`
  
  // Measure width
  let maxW = 0
  lines.value.forEach(l => {
    const w = tempCtx.measureText(l).width
    if (w > maxW) maxW = w
  })
  maxLineWidth.value = maxW
  
  // Add padding and room for effects
  const padding = 80
  const effectsRoom = (enableShadow.value ? Math.abs(shadowOffsetX.value) + shadowBlur.value : 0) + 
                    (enableOutline.value ? outlineWidth.value : 0)
  
  const targetW = Math.ceil(maxW + padding * 2 + effectsRoom)
  const targetH = Math.ceil(lines.value.length * (fontSize.value + 20) + padding * 2)
  
  // Apply bounds
  canvasWidth.value = Math.max(400, Math.min(1200, targetW))
  canvasHeight.value = Math.max(400, Math.min(1000, targetH))
}

watch([text, fontSize, enableShadow, shadowOffsetX, shadowBlur, enableOutline, outlineWidth], updateCanvasSize)

// Main drawing function
function drawFrame(frameIndex) {
  const ctx = canvas.value.getContext('2d')
  
  // Clear canvas
  ctx.fillStyle = bgColor.value
  ctx.fillRect(0, 0, canvas.value.width, canvas.value.height)
  
  // Calculate overall progress
  const overallProgress = frameIndex / totalFrames.value
  
  // Setup text rendering
  ctx.font = `${fontSize.value}px Arial`
  ctx.textBaseline = 'top'
  
  // Calculate line height with padding
  const lineHeight = fontSize.value + 20
  const totalHeight = lines.value.length * lineHeight
  const startY = (canvas.value.height - totalHeight) / 2
  
  // Setup horizontal centering base
  const startX = (canvas.value.width - maxLineWidth.value) / 2
  
  // Draw each line
  lines.value.forEach((line, lineIndex) => {
    const lineY = startY + lineIndex * lineHeight
    
    // Calculate progress for this specific line
    const charsSoFar = lines.value.slice(0, lineIndex).reduce((sum, l) => sum + l.length, 0)
    const charsThisLine = line.length
    const lineTotalChars = charsSoFar + charsThisLine
    const lineStartChars = charsSoFar
    
    // Determine how many characters to display
    let displayText = ''
    let charCount = 0
    
    if (animationType.value === 'char') {
      charCount = Math.floor(overallProgress * totalCharacters.value) - lineStartChars
      charCount = Math.max(0, Math.min(charCount, charsThisLine))
      displayText = line.substring(0, charCount)
    } 
    else if (animationType.value === 'word') {
      const words = line.split(' ')
      const wordProgress = overallProgress * totalCharacters.value - lineStartChars
      let wordCount = Math.floor(wordProgress / 5) // rough estimate
      if (wordCount > words.length) wordCount = words.length
      
      displayText = words.slice(0, wordCount).join(' ')
      if (wordCount > 0 && wordCount < words.length) {
        const nextWord = words[wordCount]
        charCount = Math.floor((wordProgress % 5) * nextWord.length / 5)
        displayText += ' ' + nextWord.substring(0, charCount)
      }
    }
    else if (animationType.value === 'fade') {
      charCount = Math.floor(overallProgress * totalCharacters.value) - lineStartChars
      charCount = Math.max(0, Math.min(charCount, charsThisLine))
      
      // Draw each character with fade effect
      ctx.globalAlpha = 1
      for (let i = 0; i < line.length; i++) {
        let opacity = 1
        if (i >= charCount) {
          opacity = 0
        }
        ctx.globalAlpha = opacity
        drawTextWithEffects(ctx, line[i], startX + ctx.measureText(line.substring(0, i)).width, lineY)
      }
      ctx.globalAlpha = 1
      displayText = line.substring(0, charCount)
    }
    
    // Draw text with effects for non-fade modes
    if (animationType.value !== 'fade') {
      drawTextWithEffects(ctx, displayText, 50, lineY)
    }
    
    // Draw cursor at the end of current line
    if (showCursor.value && lineIndex === lines.value.length - 1) {
      const cursorProgress = overallProgress % (speed.value / 1000)
      const cursorVisible = (frameIndex % 20) < 10
      if (cursorVisible && overallProgress < 0.98) {
        const cursorX = startX + ctx.measureText(displayText).width
        ctx.fillStyle = textColor.value
        ctx.globalAlpha = 0.8
        ctx.fillRect(cursorX, lineY, 3, fontSize.value)
        ctx.globalAlpha = 1
      }
    }
  })
}

function drawTextWithEffects(ctx, text, x, y) {
  ctx.save()
  
  // Apply shadow
  if (enableShadow.value) {
    ctx.shadowColor = shadowColor.value
    ctx.shadowOffsetX = shadowOffsetX.value
    ctx.shadowOffsetY = shadowOffsetX.value
    ctx.shadowBlur = shadowBlur.value
  }
  
  // Draw outline
  if (enableOutline.value) {
    ctx.strokeStyle = outlineColor.value
    ctx.lineWidth = outlineWidth.value
    ctx.strokeText(text, x, y)
  }
  
  // Apply gradient or solid fill
  if (enableGradient.value) {
    const gradient = ctx.createLinearGradient(x, y, x + ctx.measureText(text).width, y)
    gradient.addColorStop(0, gradientColor1.value)
    gradient.addColorStop(1, gradientColor2.value)
    ctx.fillStyle = gradient
  } else {
    ctx.fillStyle = textColor.value
  }
  
  ctx.fillText(text, x, y)
  
  ctx.restore()
}

function previewAnimation() {
  if (!canvas.value || !text.value) return
  
  isAnimating.value = true
  let frameIndex = 0
  
  function animate() {
    if (frameIndex < totalFrames.value) {
      drawFrame(frameIndex)
      frameIndex++
      requestAnimationFrame(animate)
    } else {
      isAnimating.value = false
    }
  }
  
  animate()
}

async function generateGif() {
  if (!canvas.value || !text.value) return
  
  isGenerating.value = true
  progress.value = 0
  
  try {
    // Load gif.js library
    await loadGifLib()
    
    // Collect all frames
    const frames = []
    for (let i = 0; i < totalFrames.value; i++) {
      drawFrame(i)
      const imageData = canvas.value.getContext('2d').getImageData(0, 0, canvas.value.width, canvas.value.height)
      frames.push(imageData)
      progress.value = Math.floor((i / totalFrames.value) * 50)
    }
    
    // Create GIF
    const gif = new GIF({
      workers: 4,
      quality: 1,
      width: canvas.value.width,
      height: canvas.value.height,
      workerScript: '/js/gif.worker.js'
    })
    
    // Add frames to GIF
    const frameDelay = 1000 / frameRate
    frames.forEach((imageData, idx) => {
      gif.addFrame(imageData, { delay: frameDelay })
      progress.value = 50 + Math.floor((idx / frames.length) * 50)
    })
    
    // Render and download
    gif.on('finished', function(blob) {
      const url = URL.createObjectURL(blob)
      const link = document.createElement('a')
      link.href = url
      link.download = `typing-animation-${Date.now()}.gif`
      document.body.appendChild(link)
      link.click()
      document.body.removeChild(link)
      URL.revokeObjectURL(url)
      
      isGenerating.value = false
      progress.value = 0
    })
    
    gif.render()
  } catch (error) {
    console.error('Error generating GIF:', error)
    alert('Error generating GIF. Check console for details.')
    isGenerating.value = false
    progress.value = 0
  }
}

function loadGifLib() {
  return new Promise((resolve, reject) => {
    if (window.GIF) {
      resolve()
      return
    }
    
    const script = document.createElement('script')
    script.src = '/js/gif.js'
    script.onload = () => resolve()
    script.onerror = () => reject(new Error('Failed to load gif.js'))
    document.head.appendChild(script)
  })
}

onMounted(() => {
  updateCanvasSize()
  previewAnimation()
})
</script>

<style scoped>
.typing-gif-generator {
  padding: 2rem 0;
}

.container {
  max-width: 900px;
  margin: 0 auto;
  padding: 0 1rem;
}

h1 {
  font-size: 28px;
  font-weight: 500;
  margin-bottom: 2rem;
  color: var(--color-text-primary);
}

h3 {
  font-size: 16px;
  font-weight: 500;
  color: var(--color-text-primary);
  margin: 0;
}

.input-section {
  background: var(--color-background-secondary);
  border-radius: var(--border-radius-lg);
  padding: 1.5rem;
  margin-bottom: 2rem;
}

.form-group {
  margin-bottom: 1.25rem;
}

.form-group label {
  display: block;
  font-size: 14px;
  font-weight: 500;
  margin-bottom: 0.5rem;
  color: var(--color-text-primary);
}

.form-group textarea,
.form-group input[type="text"],
.form-group input[type="range"],
.form-group input[type="color"],
.form-group select {
  width: 100%;
  padding: 0.75rem;
  border: 1px solid var(--color-border-tertiary);
  border-radius: var(--border-radius-md);
  font-size: 14px;
  background: var(--color-background-primary);
  color: var(--color-text-primary);
  font-family: inherit;
}

.form-group textarea {
  resize: vertical;
  font-family: var(--font-mono);
}

.form-group input[type="range"] {
  padding: 0;
  height: 6px;
  cursor: pointer;
}

.form-group input[type="checkbox"] {
  margin-right: 0.5rem;
  cursor: pointer;
  width: 18px;
  height: 18px;
}

.form-group select {
  cursor: pointer;
}

.value-display {
  display: inline-block;
  font-size: 12px;
  color: var(--color-text-secondary);
  margin-left: 0.5rem;
}

.help-text {
  display: block;
  font-size: 12px;
  color: var(--color-text-secondary);
  margin-top: 0.5rem;
}

.form-row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
}

.color-pair {
  display: flex;
  align-items: center;
  gap: 0.75rem;
}

.color-pair input[type="color"] {
  width: 50%;
  padding: 0.5rem;
}

.color-pair span {
  color: var(--color-text-secondary);
}

.effects-divider {
  border-top: 2px solid var(--color-border-tertiary);
  padding-top: 1.25rem;
  margin-top: 1.25rem;
  margin-bottom: 1.25rem;
}

.button-group {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin-top: 1.5rem;
}

.preview-btn,
.generate-btn {
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: var(--border-radius-md);
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  transition: all 0.2s ease;
}

.preview-btn {
  background: var(--color-background-secondary);
  color: var(--color-text-primary);
  border: 1px solid var(--color-border-secondary);
}

.preview-btn:hover:not(:disabled) {
  background: var(--color-border-secondary);
  border-color: var(--color-border-primary);
}

.generate-btn {
  background: #0066cc;
  color: white;
  border: none;
}

.generate-btn:hover:not(:disabled) {
  background: #0052a3;
}

.preview-btn:disabled,
.generate-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.canvas-section {
  margin-bottom: 2rem;
}

.canvas-wrapper {
  background: var(--color-background-secondary);
  border-radius: var(--border-radius-lg);
  padding: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 450px;
}

.animation-canvas {
  border: 1px solid var(--color-border-tertiary);
  border-radius: var(--border-radius-md);
  max-width: 100%;
  height: auto;
  background: white;
}

.progress {
  margin-bottom: 2rem;
  height: 28px;
  background: var(--color-background-secondary);
  border-radius: var(--border-radius-md);
  overflow: hidden;
  position: relative;
  border: 1px solid var(--color-border-tertiary);
}

.progress-bar {
  height: 100%;
  background: linear-gradient(90deg, #0066cc, #0052a3);
  transition: width 0.3s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.progress-text {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  font-weight: 500;
  color: var(--color-text-primary);
}

.info-section {
  background: var(--color-background-secondary);
  border-radius: var(--border-radius-lg);
  padding: 1.5rem;
  font-size: 14px;
  border: 1px solid var(--color-border-tertiary);
}

.info-section p {
  margin: 0.75rem 0;
  color: var(--color-text-secondary);
}

.info-section strong {
  color: var(--color-text-primary);
}

@media (max-width: 640px) {
  .form-row {
    grid-template-columns: 1fr;
  }
  
  .button-group {
    grid-template-columns: 1fr;
  }
  
  .container {
    padding: 0 0.75rem;
  }
  
  h1 {
    font-size: 24px;
  }

  .color-pair {
    flex-direction: column;
  }

  .color-pair input[type="color"] {
    width: 100%;
  }
}
</style>