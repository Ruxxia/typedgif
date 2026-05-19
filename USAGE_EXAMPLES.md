# Usage Examples - Typing Animation GIF Generator

This file shows different ways to use and customize the `TypingAnimationGif.vue` component.

## Example 1: Basic Usage (Drop-in)

The simplest way - just import and use with default settings.

```vue
<!-- pages/index.vue -->
<template>
  <div class="page">
    <TypingAnimationGif />
  </div>
</template>

<script setup>
import TypingAnimationGif from '~/components/TypingAnimationGif.vue'
</script>

<style scoped>
.page {
  max-width: 1200px;
  margin: 0 auto;
  padding: 2rem;
}
</style>
```

## Example 2: Custom Default Values

Override default settings by modifying the component.

```vue
<!-- Modified TypingAnimationGif.vue -->
<script setup>
const text = ref('Check out my portfolio!')  // Changed default
const fontSize = ref(64)  // Larger font
const speed = ref(60)  // Faster animation
const bgColor = ref('#1a1a1a')  // Dark background
const textColor = ref('#00ff00')  // Neon green
const showCursor = ref(true)
</script>
```

## Example 3: Wrapper Component with Props

Make a reusable component that accepts props.

```vue
<!-- components/TypingGifWrapper.vue -->
<template>
  <div class="wrapper">
    <h2>{{ title }}</h2>
    <TypingAnimationGif />
    <p>{{ description }}</p>
  </div>
</template>

<script setup>
import TypingAnimationGif from './TypingAnimationGif.vue'

defineProps({
  title: {
    type: String,
    default: 'Create Typing Animations'
  },
  description: {
    type: String,
    default: 'Generate beautiful GIF animations from text'
  }
})
</script>

<style scoped>
.wrapper {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 2rem;
  background: #fafafa;
}

h2 {
  margin: 0 0 1rem 0;
  color: #333;
}

p {
  margin: 1rem 0 0 0;
  color: #666;
  font-size: 14px;
}
</style>
```

## Example 4: Multiple Instances

Use the component multiple times with different configurations.

```vue
<!-- pages/gallery.vue -->
<template>
  <div class="gallery">
    <div class="item">
      <h3>Code Tutorial Animation</h3>
      <TypingAnimationGif :config="codeConfig" />
    </div>
    
    <div class="item">
      <h3>Social Media Promo</h3>
      <TypingAnimationGif :config="socialConfig" />
    </div>
    
    <div class="item">
      <h3>Product Demo</h3>
      <TypingAnimationGif :config="demoConfig" />
    </div>
  </div>
</template>

<script setup>
const codeConfig = {
  text: 'console.log("Hello World")',
  fontSize: 48,
  bgColor: '#282c34',
  textColor: '#61dafb'
}

const socialConfig = {
  text: 'Follow me for more tips!',
  fontSize: 56,
  bgColor: '#ff1493',
  textColor: '#ffffff'
}

const demoConfig = {
  text: 'Amazing new feature unlocked!',
  fontSize: 52,
  bgColor: '#4a90e2',
  textColor: '#ffffff'
}
</script>

<style scoped>
.gallery {
  display: grid;
  grid-template-columns: 1fr;
  gap: 2rem;
  max-width: 1000px;
  margin: 0 auto;
}

.item {
  border: 1px solid #eee;
  border-radius: 8px;
  padding: 1.5rem;
}

h3 {
  margin: 0 0 1rem 0;
}
</style>
```

## Example 5: Blog Post Integration

Embed in a blog post or article.

```vue
<!-- pages/blog/[slug].vue -->
<template>
  <article class="blog-post">
    <h1>{{ post.title }}</h1>
    <div class="meta">
      <span>{{ post.author }}</span>
      <span>{{ post.date }}</span>
    </div>
    
    <div class="content">
      <p>Here's an example of the animation in action:</p>
      
      <div class="animation-embed">
        <TypingAnimationGif />
      </div>
      
      <p>{{ post.content }}</p>
    </div>
  </article>
</template>

<script setup>
import TypingAnimationGif from '~/components/TypingAnimationGif.vue'

const post = {
  title: 'Creating Awesome Typing Animations',
  author: 'John Doe',
  date: '2024-01-15',
  content: 'Typing animations are a great way to grab attention...'
}
</script>

<style scoped>
.blog-post {
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
}

.meta {
  color: #666;
  font-size: 14px;
  margin-bottom: 2rem;
}

.meta span {
  margin-right: 1rem;
}

.animation-embed {
  margin: 2rem 0;
  padding: 1.5rem;
  background: #f5f5f5;
  border-radius: 8px;
}

.content p {
  line-height: 1.6;
  margin: 1rem 0;
}
</style>
```

## Example 6: Portfolio/Resume Section

Show typing animation as part of a portfolio.

```vue
<!-- components/PortfolioHero.vue -->
<template>
  <section class="hero">
    <div class="intro">
      <h1>Welcome to My Portfolio</h1>
      <p>I create beautiful interactive experiences</p>
    </div>
    
    <div class="animation-showcase">
      <TypingAnimationGif />
    </div>
    
    <div class="cta">
      <button>View My Work</button>
      <button class="secondary">Contact Me</button>
    </div>
  </section>
</template>

<script setup>
import TypingAnimationGif from './TypingAnimationGif.vue'
</script>

<style scoped>
.hero {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 3rem;
  padding: 2rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
}

.intro {
  text-align: center;
}

.intro h1 {
  font-size: 3rem;
  margin: 0 0 1rem 0;
}

.intro p {
  font-size: 1.2rem;
  opacity: 0.9;
}

.animation-showcase {
  max-width: 900px;
  width: 100%;
}

.cta {
  display: flex;
  gap: 1rem;
}

button {
  padding: 1rem 2rem;
  border: none;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: 500;
  cursor: pointer;
  background: white;
  color: #667eea;
}

button.secondary {
  background: transparent;
  color: white;
  border: 2px solid white;
}

button:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 20px rgba(0,0,0,0.2);
}
</style>
```

## Example 7: Composable Version

Extract logic into a composable for reuse.

```javascript
// composables/useTypingGif.js
import { ref, computed } from 'vue'

export function useTypingGif(initialText = 'Hello World!') {
  const text = ref(initialText)
  const animationType = ref('char')
  const speed = ref(80)
  const fontSize = ref(48)
  const textColor = ref('#000000')
  const bgColor = ref('#ffffff')
  const showCursor = ref(true)
  const isGenerating = ref(false)
  const progress = ref(0)
  
  const animationDuration = computed(() => {
    return (text.value.length * speed.value / 1000).toFixed(2)
  })
  
  const totalFrames = computed(() => {
    return Math.ceil((text.value.length * speed.value / 1000) * 30)
  })
  
  const estimatedSize = computed(() => {
    const frameSize = (800 * 300 * 4) / 1024
    const totalSize = frameSize * totalFrames.value
    return (totalSize / 1024).toFixed(1) + ' KB'
  })
  
  async function generateGif(canvasRef) {
    // Implementation here
  }
  
  return {
    text,
    animationType,
    speed,
    fontSize,
    textColor,
    bgColor,
    showCursor,
    isGenerating,
    progress,
    animationDuration,
    totalFrames,
    estimatedSize,
    generateGif
  }
}
```

Usage:
```vue
<script setup>
import { useTypingGif } from '~/composables/useTypingGif'

const gif = useTypingGif('Custom text')
</script>

<template>
  <input v-model="gif.text" />
  <button @click="gif.generateGif(canvasRef)">Generate</button>
</template>
```

## Example 8: Admin Dashboard Widget

Use as a tool in an admin panel.

```vue
<!-- components/admin/TextAnimationTool.vue -->
<template>
  <div class="widget">
    <div class="widget-header">
      <h3>GIF Generator Tool</h3>
      <button @click="showSettings = !showSettings" class="settings-btn">
        ⚙️ Settings
      </button>
    </div>
    
    <TypingAnimationGif v-if="!showSettings" />
    
    <div v-if="showSettings" class="settings-panel">
      <h4>Generator Settings</h4>
      <div class="setting">
        <label>Animation Type</label>
        <select>
          <option>Character</option>
          <option>Word</option>
          <option>Fade</option>
        </select>
      </div>
      
      <div class="setting">
        <label>Export Format</label>
        <select>
          <option>GIF</option>
          <option>MP4 (beta)</option>
          <option>WebP</option>
        </select>
      </div>
      
      <button @click="showSettings = false" class="close-btn">
        Back to Generator
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import TypingAnimationGif from '../TypingAnimationGif.vue'

const showSettings = ref(false)
</script>

<style scoped>
.widget {
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
  overflow: hidden;
}

.widget-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 1rem;
  border-bottom: 1px solid #eee;
}

.settings-btn {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 1.2rem;
}

.settings-panel {
  padding: 1.5rem;
}

.setting {
  margin-bottom: 1.5rem;
}

.setting label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: 500;
}

.setting select {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.close-btn {
  width: 100%;
  padding: 0.75rem;
  background: #0066cc;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: 500;
}
</style>
```

## Example 9: Mobile-Optimized Version

Smaller canvas for mobile devices.

```vue
<!-- pages/mobile-generator.vue -->
<template>
  <div class="mobile-container">
    <h1>Mobile GIF Maker</h1>
    
    <div class="canvas-mobile">
      <canvas 
        ref="canvas"
        :width="isMobile ? 400 : 800"
        :height="isMobile ? 200 : 300"
      ></canvas>
    </div>
    
    <div class="controls-mobile">
      <input v-model="text" placeholder="Enter text...">
      <button @click="generateGif">Create GIF</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const isMobile = computed(() => {
  return window.innerWidth < 768
})
</script>

<style scoped>
.mobile-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 1rem;
}

.canvas-mobile {
  margin: 1.5rem 0;
  display: flex;
  justify-content: center;
}

.controls-mobile {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

input {
  padding: 0.75rem;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 1rem;
}

button {
  padding: 1rem;
  background: #0066cc;
  color: white;
  border: none;
  border-radius: 6px;
  font-size: 1rem;
  font-weight: 500;
}
</style>
```

## Example 10: API Integration

Use with a backend API to store animations.

```javascript
// utils/animationApi.js
export async function saveAnimation(gifData, metadata) {
  const formData = new FormData()
  formData.append('file', gifData, 'animation.gif')
  formData.append('metadata', JSON.stringify(metadata))
  
  const response = await fetch('/api/animations', {
    method: 'POST',
    body: formData
  })
  
  return response.json()
}

export async function getAnimations() {
  const response = await fetch('/api/animations')
  return response.json()
}

export async function deleteAnimation(id) {
  const response = await fetch(`/api/animations/${id}`, {
    method: 'DELETE'
  })
  return response.json()
}
```

Usage in component:
```vue
<script setup>
import { saveAnimation } from '~/utils/animationApi'

async function generateAndSave() {
  const gif = await generateGif()
  const result = await saveAnimation(gif, {
    text: text.value,
    speed: speed.value,
    timestamp: new Date()
  })
  console.log('Saved:', result)
}
</script>
```

## Example 11: Dark Mode Support

Add dark mode toggle.

```vue
<template>
  <div :class="{ dark: isDark }">
    <button @click="isDark = !isDark">
      {{ isDark ? '☀️' : '🌙' }} Toggle Theme
    </button>
    
    <TypingAnimationGif />
  </div>
</template>

<script setup>
import { ref, watch } from 'vue'
import TypingAnimationGif from '~/components/TypingAnimationGif.vue'

const isDark = ref(false)

watch(isDark, (newVal) => {
  if (newVal) {
    document.documentElement.classList.add('dark')
  } else {
    document.documentElement.classList.remove('dark')
  }
})
</script>

<style scoped>
:deep(.dark) {
  --color-background-primary: #1a1a1a;
  --color-background-secondary: #2d2d2d;
  --color-text-primary: #ffffff;
  --color-text-secondary: #b0b0b0;
}

button {
  padding: 0.5rem 1rem;
  margin-bottom: 1rem;
  background: var(--color-background-secondary);
  color: var(--color-text-primary);
  border: 1px solid var(--color-border-tertiary);
  border-radius: 6px;
  cursor: pointer;
}
</style>
```

## Tips for All Examples

1. **Always import the component** before using it
2. **Use scoped styles** to avoid CSS conflicts
3. **Handle loading states** for better UX
4. **Add error handling** for gif.js library loading
5. **Test on mobile** for responsive design
6. **Cache canvas references** for performance
7. **Consider lazy loading** for multiple instances

---

Pick any example above and adapt it to your needs. Start simple and add complexity as needed!
