<script setup>
import { ref, onMounted } from 'vue'

const emit = defineEmits(['opened'])
const phase = ref('initial') // initial → splitting → done

onMounted(() => {
  setTimeout(() => {
    phase.value = 'splitting'
    setTimeout(() => {
      phase.value = 'done'
      emit('opened')
    }, 2500)
  }, 800)
})
</script>

<template>
  <div class="envelope-overlay" :class="{ 'fade-out': phase === 'done' }">
    <div class="envelope-scene">
      <!-- Card revealed beneath the envelope halves -->
      <div class="card-peek" :class="{ visible: phase === 'splitting' || phase === 'done' }">
        <div class="card-peek-inner">
          <p class="card-peek-script">Grâce &amp; Robinson</p>
          <div class="card-peek-divider"></div>
          <p class="card-peek-subtitle">Mariage Coutumier · DOT</p>
          <p class="card-peek-date">30 Juillet 2026</p>
        </div>
      </div>

      <!-- Left half of envelope -->
      <div class="envelope-half left" :class="{ 'slide-left': phase === 'splitting' || phase === 'done' }">
        <div class="half-content left-content">
          <!-- Envelope body with flap detail -->
          <div class="flap-detail top-left"></div>
          <div class="flap-detail bottom-left"></div>
          <div class="envelope-pattern">
            <div class="wave-line" v-for="i in 6" :key="i"></div>
          </div>
          <div class="seal-half seal-left">
            <span class="seal-letter">G</span>
          </div>
        </div>
      </div>

      <!-- Right half of envelope -->
      <div class="envelope-half right" :class="{ 'slide-right': phase === 'splitting' || phase === 'done' }">
        <div class="half-content right-content">
          <div class="flap-detail top-right"></div>
          <div class="flap-detail bottom-right"></div>
          <div class="envelope-pattern">
            <div class="wave-line" v-for="i in 6" :key="i"></div>
          </div>
          <div class="seal-half seal-right">
            <span class="seal-letter">R</span>
          </div>
        </div>
      </div>

      <!-- Envelope flap (top) -->
      <div class="envelope-flap" :class="{ 'flap-open': phase === 'splitting' || phase === 'done' }">
        <div class="flap-inner">
          <div class="flap-ornament">✦</div>
        </div>
      </div>

      <!-- Opening hint -->
      <div class="hint-text" :class="{ hidden: phase !== 'initial' }">
        <div class="hint-pulse">
          <span>✉</span>
        </div>
        <p>Votre invitation s'ouvre…</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.envelope-overlay {
  position: fixed;
  inset: 0;
  z-index: 1000;
  background: radial-gradient(ellipse at center, #2a1a08 0%, #1a0e04 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  transition: opacity 0.8s ease 0.4s;
}

.envelope-overlay.fade-out {
  opacity: 0;
  pointer-events: none;
}

.envelope-scene {
  position: relative;
  width: 420px;
  height: 300px;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* ── Card peek ── */
.card-peek {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1;
  opacity: 0;
  transform: scale(0.92);
  transition: opacity 0.6s ease 0.3s, transform 0.6s ease 0.3s;
}

.card-peek.visible {
  opacity: 1;
  transform: scale(1);
}

.card-peek-inner {
  background: linear-gradient(160deg, var(--color-cream) 0%, var(--color-cream-dark) 100%);
  border: 2px solid var(--color-gold-light);
  border-radius: 4px;
  padding: 2rem 3rem;
  text-align: center;
  box-shadow: 0 8px 40px rgba(0, 0, 0, 0.5);
  width: 320px;
}

.card-peek-script {
  font-family: var(--font-script);
  font-size: 2.4rem;
  color: var(--color-brown-dark);
  line-height: 1.2;
}

.card-peek-divider {
  width: 60px;
  height: 1px;
  background: var(--color-gold);
  margin: 0.6rem auto;
}

.card-peek-subtitle {
  font-family: var(--font-serif);
  font-size: 0.85rem;
  color: var(--color-brown);
  letter-spacing: 0.1em;
  text-transform: uppercase;
}

.card-peek-date {
  font-family: var(--font-serif);
  font-size: 1rem;
  color: var(--color-brown-dark);
  margin-top: 0.4rem;
  font-weight: 600;
}

/* ── Envelope halves ── */
.envelope-half {
  position: absolute;
  top: 0;
  width: 50%;
  height: 100%;
  z-index: 2;
  transition: transform 1.4s cubic-bezier(0.77, 0, 0.18, 1) 0.1s,
              opacity 0.4s ease 1.8s;
  overflow: hidden;
}

.envelope-half.left {
  left: 0;
  border-radius: 8px 0 0 8px;
}

.envelope-half.right {
  right: 0;
  border-radius: 0 8px 8px 0;
}

.envelope-half.slide-left {
  transform: translateX(-120%);
}

.envelope-half.slide-right {
  transform: translateX(120%);
}

.half-content {
  width: 100%;
  height: 100%;
  background: linear-gradient(160deg, #6b4423 0%, #5C3D1E 60%, #3e2710 100%);
  position: relative;
  overflow: hidden;
}

/* Border detail for envelope */
.half-content::before {
  content: '';
  position: absolute;
  inset: 4px;
  border: 1px solid rgba(212, 175, 55, 0.35);
  pointer-events: none;
}

/* Inside lighter part */
.half-content::after {
  content: '';
  position: absolute;
  top: 0;
  bottom: 0;
  width: 12px;
  background: linear-gradient(90deg, rgba(255,248,231,0.15), transparent);
}

.left-content::after { right: 0; }
.right-content::after { left: 0; background: linear-gradient(270deg, rgba(255,248,231,0.15), transparent); }

/* Flap detail marks */
.flap-detail {
  position: absolute;
  border: 1px solid rgba(212, 175, 55, 0.25);
}

.top-left {
  top: 8px;
  left: 8px;
  right: 0;
  bottom: 50%;
  border-bottom: none;
  border-right: none;
}

.bottom-left {
  bottom: 8px;
  left: 8px;
  right: 0;
  top: 50%;
  border-top: none;
  border-right: none;
}

.top-right {
  top: 8px;
  right: 8px;
  left: 0;
  bottom: 50%;
  border-bottom: none;
  border-left: none;
}

.bottom-right {
  bottom: 8px;
  right: 8px;
  left: 0;
  top: 50%;
  border-top: none;
  border-left: none;
}

/* Wave / pattern lines */
.envelope-pattern {
  position: absolute;
  inset: 20px;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  opacity: 0.18;
}

.wave-line {
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--color-gold), transparent);
  border-radius: 1px;
}

/* Wax seal halves */
.seal-half {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: radial-gradient(circle at 35% 35%, #b8860b, #8B6914);
  border: 2px solid var(--color-gold);
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 2px 12px rgba(0,0,0,0.4);
}

.seal-left { right: -24px; }
.seal-right { left: -24px; }

.seal-letter {
  font-family: var(--font-script);
  font-size: 1.5rem;
  color: var(--color-cream);
  text-shadow: 0 1px 4px rgba(0,0,0,0.3);
}

/* ── Envelope flap ── */
.envelope-flap {
  position: absolute;
  top: -4px;
  left: 0;
  right: 0;
  height: 52%;
  z-index: 3;
  clip-path: polygon(0 0, 100% 0, 50% 100%);
  background: linear-gradient(175deg, #7a4f28 0%, #5C3D1E 100%);
  transform-origin: top center;
  transition: transform 0.6s ease, opacity 0.4s ease 2s;
  box-shadow: 0 4px 20px rgba(0,0,0,0.3);
}

.envelope-flap::before {
  content: '';
  position: absolute;
  inset: 2px;
  clip-path: polygon(2% 2%, 98% 2%, 50% 95%);
  border: 1px solid rgba(212, 175, 55, 0.3);
  pointer-events: none;
}

.envelope-flap.flap-open {
  transform: rotateX(180deg) translateY(-10px);
  opacity: 0;
}

.flap-inner {
  display: flex;
  align-items: center;
  justify-content: center;
  padding-top: 1rem;
}

.flap-ornament {
  color: var(--color-gold-light);
  font-size: 1.2rem;
  opacity: 0.7;
}

/* ── Hint text ── */
.hint-text {
  position: absolute;
  bottom: -80px;
  left: 50%;
  transform: translateX(-50%);
  text-align: center;
  color: rgba(255, 248, 231, 0.7);
  transition: opacity 0.5s ease;
  white-space: nowrap;
}

.hint-text.hidden {
  opacity: 0;
}

.hint-pulse {
  font-size: 2rem;
  animation: pulse 1.5s ease-in-out infinite;
  margin-bottom: 0.4rem;
}

.hint-text p {
  font-family: var(--font-serif);
  font-size: 0.85rem;
  font-style: italic;
  letter-spacing: 0.05em;
}

@keyframes pulse {
  0%, 100% { transform: scale(1); opacity: 0.7; }
  50% { transform: scale(1.15); opacity: 1; }
}

/* ── Responsive ── */
@media (max-width: 480px) {
  .envelope-scene {
    width: 310px;
    height: 220px;
  }

  .card-peek-inner {
    width: 240px;
    padding: 1.5rem 2rem;
  }

  .card-peek-script {
    font-size: 1.8rem;
  }
}
</style>
