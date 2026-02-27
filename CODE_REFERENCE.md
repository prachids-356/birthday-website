# 💕 3D Heart Animation Code Reference

## Key JavaScript Functions

### 1. Background Ambient Hearts
```javascript
function createBackgroundHearts() {
    const bgHearts = document.getElementById('bgHearts');
    for (let i = 0; i < 12; i++) {
        const heart = document.createElement('div');
        heart.className = 'floating-3d-heart';
        heart.innerHTML = '💕';
        heart.style.left = Math.random() * 100 + '%';
        heart.style.top = Math.random() * 100 + '%';
        heart.style.animationDelay = (i * 1.5) + 's';
        heart.style.fontSize = (30 + Math.random() * 30) + 'px';
        bgHearts.appendChild(heart);
    }
}
```
**Called**: On page load + random scroll events
**Creates**: Soft floating background animation

---

### 2. Exploding Hearts (Main Animation)
```javascript
function createHearts() {
    const animationTypes = ['heart3DExplode', 'heart-float-3d', 
                          'heart-spin-3d', 'heartbeat-glow'];
    
    for (let i = 0; i < 12; i++) {
        setTimeout(() => {
            const heart = document.createElement('div');
            heart.className = 'heart-3d';
            
            // Random animation type
            const animationType = animationTypes[
                Math.floor(Math.random() * animationTypes.length)
            ];
            const innerDiv = document.createElement('div');
            innerDiv.className = animationType;
            innerDiv.innerHTML = '💕';
            innerDiv.style.fontSize = '30px';
            
            heart.appendChild(innerDiv);
            heart.style.left = Math.random() * window.innerWidth + 'px';
            heart.style.top = Math.random() * window.innerHeight + 'px';
            
            // Random explosion direction
            const angle = Math.random() * Math.PI * 2;
            const distance = 200 + Math.random() * 300;
            const tx = Math.cos(angle) * distance;
            const ty = Math.sin(angle) * distance;
            innerDiv.style.setProperty('--tx', tx + 'px');
            innerDiv.style.setProperty('--ty', ty + 'px');
            
            document.body.appendChild(heart);
            setTimeout(() => heart.remove(), 5000);
        }, i * 100);  // 100ms stagger
    }
}
```
**Called**: Gallery clicks, message clicks, love message popup
**Creates**: 12 staggered 3D hearts with varied animations

---

### 3. Falling Confetti
```javascript
function createConfetti() {
    const confettiCount = 15;
    for (let i = 0; i < confettiCount; i++) {
        setTimeout(() => {
            const confetti = document.createElement('div');
            confetti.className = 'confetti-heart';
            confetti.innerHTML = '💕';
            confetti.style.left = Math.random() * window.innerWidth + 'px';
            confetti.style.top = '-50px';
            confetti.style.fontSize = (20 + Math.random() * 30) + 'px';
            
            // Random X offset
            const xOffset = (Math.random() - 0.5) * 400;
            confetti.style.setProperty('--confetti-x', xOffset + 'px');
            
            document.body.appendChild(confetti);
            setTimeout(() => confetti.remove(), 4000);
        }, i * 80);  // 80ms stagger
    }
}
```
**Called**: After gallery/message clicks and popup opening
**Creates**: 15 cascading falling hearts with spin

---

### 4. Burst from Click Point
```javascript
function createBurst(x, y) {
    const burstCount = 20;
    for (let i = 0; i < burstCount; i++) {
        const burst = document.createElement('div');
        burst.className = 'love-burst';
        burst.innerHTML = '💕';
        burst.style.left = x + 'px';
        burst.style.top = y + 'px';
        burst.style.fontSize = (20 + Math.random() * 25) + 'px';
        
        document.body.appendChild(burst);
        setTimeout(() => burst.remove(), 2500);
    }
}
```
**Called**: Gallery clicks, message clicks with coordinates
**Creates**: 20 hearts expanding outward from click point
**Effect**: Explosive burst from interaction location

---

### 5. Particle Spread
```javascript
function createParticles(e) {
    for (let i = 0; i < 6; i++) {
        const particle = document.createElement('div');
        particle.className = 'heart-particle';
        particle.innerHTML = '💕';
        particle.style.left = e.clientX + 'px';
        particle.style.top = e.clientY + 'px';
        
        // 6-way radial spread
        const angle = (i / 6) * Math.PI * 2;
        const distance = 100;
        const tx = Math.cos(angle) * distance;
        const ty = Math.sin(angle) * distance;
        
        particle.style.setProperty('--tx', tx + 'px');
        particle.style.setProperty('--ty', ty + 'px');
        
        document.body.appendChild(particle);
        setTimeout(() => particle.remove(), 2000);
    }
}
```
**Called**: Message item clicks
**Creates**: 6 hearts in radial pattern
**Effect**: Particles float outward in perfect circle

---

## Key CSS Animations

### 3D Explosion
```css
@keyframes heart3DExplode {
    0% {
        opacity: 1;
        transform: translate(0, 0) rotateX(0) rotateY(0) rotateZ(0) scale(1);
        filter: drop-shadow(0 0 20px rgba(255,20,147,1));
    }
    50% {
        opacity: 1;
        filter: drop-shadow(0 0 40px rgba(255,20,147,1));
    }
    100% {
        opacity: 0;
        transform: translate(var(--tx), var(--ty)) rotateX(720deg) rotateY(720deg) rotateZ(360deg) scale(0);
        filter: drop-shadow(0 0 10px rgba(255,20,147,0.2));
    }
}
```
**Key Points**:
- Uses CSS variables (--tx, --ty) for dynamic direction
- Triple-axis rotation (720°, 720°, 360°) = 2+ full spins
- Scale to 0 creates shrinking effect
- Glow pulses at midpoint

---

### Floating 3D
```css
@keyframes heart3DFloat {
    0%, 100% {
        transform: translateY(0) rotateX(0) rotateY(0) rotateZ(0);
        filter: drop-shadow(0 0 10px rgba(255,20,147,0.5));
    }
    25% {
        transform: translateY(-30px) rotateX(45deg) rotateY(45deg);
        filter: drop-shadow(0 0 20px rgba(255,20,147,0.8));
    }
    50% {
        transform: translateY(-60px) rotateX(90deg) rotateY(90deg) rotateZ(90deg);
        filter: drop-shadow(0 0 30px rgba(255,20,147,1));
    }
    75% {
        transform: translateY(-30px) rotateX(-45deg) rotateY(-45deg);
        filter: drop-shadow(0 0 20px rgba(255,20,147,0.8));
    }
}
```
**Key Points**:
- Floats upward 60px max
- 3D tilting on X and Y axes
- Glow intensifies toward peak
- Ease-in-out timing = smooth motion

---

### Love Burst
```css
@keyframes loveBurst {
    0% {
        opacity: 1;
        transform: scale(0.5) rotateX(0) rotateY(0) rotateZ(0);
        filter: drop-shadow(0 0 10px rgba(255,20,147,0.5));
    }
    25% {
        opacity: 1;
        transform: scale(1) rotateX(90deg) rotateY(90deg) rotateZ(90deg);
        filter: drop-shadow(0 0 30px rgba(255,20,147,1));
    }
    50% {
        opacity: 1;
        transform: scale(1.5) rotateX(180deg) rotateY(180deg) rotateZ(180deg);
        filter: drop-shadow(0 0 40px rgba(255,20,147,1));
    }
    75% {
        opacity: 0.5;
        transform: scale(2) rotateX(270deg) rotateY(270deg) rotateZ(270deg);
        filter: drop-shadow(0 0 30px rgba(255,20,147,0.6));
    }
    100% {
        opacity: 0;
        transform: scale(3) rotateX(360deg) rotateY(360deg) rotateZ(360deg);
        filter: drop-shadow(0 0 5px rgba(255,20,147,0.1));
    }
}
```
**Key Points**:
- Scales from 0.5 to 3 (6x expansion)
- All axes rotate 360°+
- cubic-bezier easing = elastic bounce
- Glow peaks at 40px mid-animation

---

## HTML Structure

### Background Hearts Container
```html
<div class="bg-hearts" id="bgHearts"></div>
```
**Purpose**: Fixed container for ambient floating hearts
**Z-index**: 2 (behind main content)

### Gallery with Click Handlers
```html
<div class="gallery-item" onclick="
    createHearts(); 
    createHearts(); 
    createConfetti(); 
    createBurst(this.getBoundingClientRect().left + 50, 
               this.getBoundingClientRect().top + 50);">
    <img src="image1.jpg">
    <div class="gallery-overlay">💕</div>
</div>
```
**Triggers**: All 4 animation types on single click
**Coordinates**: Burst centers on photo location

### Message Items
```html
<div class="message-item" onclick="
    createHearts(); 
    createHearts(); 
    createConfetti(); 
    createBurst(e.clientX, e.clientY); 
    createParticles(e);">
    <strong>1.</strong> Reason text
</div>
```
**Triggers**: All 5 animation types
**Event**: Passes event object for particle positioning

### Love Message Button
```html
<button onclick="showLoveMessage()" style="...">
    💕 OPEN LOVE MESSAGE
</button>
```
**Inside showLoveMessage()**:
```javascript
for (let i = 0; i < 3; i++) {
    setTimeout(() => {
        createHearts();
        createConfetti();
        createBurst(window.innerWidth / 2, window.innerHeight / 2);
    }, i * 300);
}
```
**Effect**: Triple-wave burst from center

---

## CSS Classes & Their Animations

| Class | Animation | Duration | Effect |
|-------|-----------|----------|--------|
| `.floating-3d-heart` | `float3D` | 15s | Ambient floating |
| `.heart-3d-content` | `heart3DExplode` | 4s | Explosive spin |
| `.heartbeat-glow` | `heartbeatGlow` | 1s loop | Pulsing glow |
| `.heart-pulse-big` | `pulseBig` | 1.5s loop | Large pulse |
| `.heart-spin-3d` | `heart3DSpin` | 2.5s loop | Smooth spin |
| `.heart-float-3d` | `heart3DFloat` | 4s loop | Floating motion |
| `.heart-bubble` | `orbitHearts` | 8s loop | Circular orbit |
| `.confetti-heart` | `confetti` | 4s | Falling spin |
| `.love-burst` | `loveBurst` | 2.5s | Expanding burst |
| `.heart-particle` | `particleFloat` | 2s | Radial spread |

---

## Common Patterns

### Random 3D Rotations
```javascript
const angle = Math.random() * Math.PI * 2;  // 0-360°
const distance = 200 + Math.random() * 300;  // 200-500px
const tx = Math.cos(angle) * distance;       // X offset
const ty = Math.sin(angle) * distance;       // Y offset
element.style.setProperty('--tx', tx + 'px');
element.style.setProperty('--ty', ty + 'px');
```
**Creates**: Procedural directional explosion

### Staggered Timing
```javascript
setTimeout(() => {
    // Create heart
}, i * 100);  // Each heart offset by 100ms
```
**Creates**: Cascading wave effect

### Animation Type Randomization
```javascript
const types = ['anim1', 'anim2', 'anim3', 'anim4'];
const type = types[Math.floor(Math.random() * types.length)];
innerDiv.className = type;  // Assign random animation
```
**Creates**: Visual variety from same function

---

## Performance Tips

### Memory Management
```javascript
setTimeout(() => heart.remove(), 5000);  // Auto cleanup
```
After animation completes, element is removed to prevent memory leaks

### GPU Acceleration
All transforms use CSS (GPU-accelerated):
- `transform: translate()` ✅ Fast
- `transform: rotate()` ✅ Fast
- `filter: drop-shadow()` ✅ Fast
- `opacity` ✅ Fast

Avoid non-accelerated properties:
- `left`, `top` ❌ Slow (triggers reflow)
- `width`, `height` ❌ Slow (triggers reflow)

### Cleanup
```javascript
document.body.appendChild(heart);      // Add element
setTimeout(() => heart.remove(), 5000); // Remove after animation
```
Prevents DOM bloat from accumulating hearts

---

## Customization Examples

### Slow down explosions
```javascript
// Original: 4s
animation: heart3DExplode 6s cubic-bezier(...) forwards;
```

### Change burst size
```javascript
// Original: scale(3)
// Change to scale(5) for bigger burst
transform: scale(5) ...
```

### Modify glow color
```javascript
// Original: rgba(255,20,147,1)  [Hot pink]
// Change to rgba(0,212,255,1)   [Cyan]
filter: drop-shadow(0 0 20px rgba(0,212,255,1));
```

### Increase confetti count
```javascript
// Original: 15
const confettiCount = 25;  // More hearts falling
```

---

## Browser Compatibility

✅ **Full Support**:
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

**Required Features**:
- CSS 3D Transforms (rotateX, rotateY, rotateZ)
- CSS Custom Properties (--variables)
- Filter: drop-shadow()
- CSS Animations (@keyframes)

**Fallback**: Hearts still appear but without 3D effects in older browsers

