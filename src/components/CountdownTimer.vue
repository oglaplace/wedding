<script setup>
import { ref, onMounted, onUnmounted } from 'vue'
import { WEDDING_CONFIG } from '../config/index.js'

const days = ref(0)
const hours = ref(0)
const minutes = ref(0)
const seconds = ref(0)
const expired = ref(false)

let timer = null

function updateCountdown() {
  const now = Date.now()
  const target = WEDDING_CONFIG.date.getTime()
  const diff = target - now

  if (diff <= 0) {
    days.value = 0
    hours.value = 0
    minutes.value = 0
    seconds.value = 0
    expired.value = true
    clearInterval(timer)
    return
  }

  days.value = Math.floor(diff / (1000 * 60 * 60 * 24))
  hours.value = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60))
  minutes.value = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60))
  seconds.value = Math.floor((diff % (1000 * 60)) / 1000)
}

onMounted(() => {
  updateCountdown()
  timer = setInterval(updateCountdown, 1000)
})

onUnmounted(() => {
  clearInterval(timer)
})

function pad(n) {
  return String(n).padStart(2, '0')
}
</script>

<template>
  <div class="countdown-wrapper">
    <!-- Title -->
    <div class="countdown-header">
      <p class="countdown-script">Le Grand Jour</p>
      <h2 class="countdown-title">Compte à rebours</h2>
      <p class="countdown-date-label">{{ WEDDING_CONFIG.dateDisplay }} · {{ WEDDING_CONFIG.time }}</p>
      <div class="divider-light"></div>
    </div>

    <!-- Expired message -->
    <div v-if="expired" class="expired-message">
      <p class="expired-script">Le grand jour est arrivé !</p>
      <p>Félicitations à Grâce &amp; Robinson 🎉</p>
    </div>

    <!-- Countdown units -->
    <div v-else class="countdown-grid">
      <div class="count-unit">
        <div class="count-box">
          <span class="count-number">{{ pad(days) }}</span>
        </div>
        <span class="count-label">Jours</span>
      </div>

      <div class="count-separator">:</div>

      <div class="count-unit">
        <div class="count-box">
          <span class="count-number">{{ pad(hours) }}</span>
        </div>
        <span class="count-label">Heures</span>
      </div>

      <div class="count-separator">:</div>

      <div class="count-unit">
        <div class="count-box">
          <span class="count-number">{{ pad(minutes) }}</span>
        </div>
        <span class="count-label">Minutes</span>
      </div>

      <div class="count-separator">:</div>

      <div class="count-unit">
        <div class="count-box">
          <span class="count-number">{{ pad(seconds) }}</span>
        </div>
        <span class="count-label">Secondes</span>
      </div>
    </div>
  </div>
</template>

<style scoped>
.countdown-wrapper {
  max-width: 800px;
  margin: 0 auto;
  text-align: center;
}

/* ── Header ── */
.countdown-header {
  margin-bottom: 2.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.4rem;
}

.countdown-script {
  font-family: var(--font-script);
  font-size: 2.5rem;
  color: var(--color-gold);
  line-height: 1.1;
}

.countdown-title {
  font-family: var(--font-serif);
  font-size: 1.6rem;
  color: var(--color-cream);
  font-weight: 600;
  letter-spacing: 0.05em;
}

.countdown-date-label {
  font-family: var(--font-serif);
  font-size: 0.9rem;
  color: rgba(255, 248, 231, 0.65);
  font-style: italic;
}

.divider-light {
  width: 60px;
  height: 1px;
  background: linear-gradient(90deg, transparent, var(--color-gold), transparent);
  margin-top: 0.6rem;
}

/* ── Grid ── */
.countdown-grid {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  flex-wrap: wrap;
}

.count-unit {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.6rem;
}

.count-box {
  background: rgba(255, 255, 255, 0.07);
  border: 1px solid rgba(212, 175, 55, 0.4);
  border-radius: 8px;
  width: 110px;
  height: 110px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 4px 20px rgba(0,0,0,0.25), inset 0 1px 0 rgba(212, 175, 55, 0.2);
  position: relative;
  overflow: hidden;
}

.count-box::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 50%;
  background: rgba(255,255,255,0.04);
  border-bottom: 1px solid rgba(212, 175, 55, 0.15);
}

.count-number {
  font-family: var(--font-serif);
  font-size: 2.8rem;
  font-weight: 700;
  color: var(--color-gold);
  letter-spacing: -0.02em;
  position: relative;
  z-index: 1;
  text-shadow: 0 2px 10px rgba(0,0,0,0.3);
}

.count-label {
  font-family: var(--font-sans);
  font-size: 0.7rem;
  text-transform: uppercase;
  letter-spacing: 0.15em;
  color: rgba(255, 248, 231, 0.6);
  font-weight: 500;
}

.count-separator {
  font-size: 2.5rem;
  color: var(--color-gold-light);
  opacity: 0.5;
  font-weight: 700;
  margin-bottom: 1.6rem;
  align-self: flex-start;
  margin-top: 0.8rem;
}

/* ── Expired ── */
.expired-message {
  color: var(--color-cream);
  text-align: center;
  padding: 2rem;
}

.expired-script {
  font-family: var(--font-script);
  font-size: 2.5rem;
  color: var(--color-gold);
  margin-bottom: 0.5rem;
}

/* ── Responsive ── */
@media (max-width: 600px) {
  .count-box {
    width: 75px;
    height: 75px;
  }

  .count-number {
    font-size: 2rem;
  }

  .countdown-grid {
    gap: 0.25rem;
  }

  .count-separator {
    font-size: 1.8rem;
  }
}

@media (max-width: 380px) {
  .count-box {
    width: 60px;
    height: 60px;
  }

  .count-number {
    font-size: 1.6rem;
  }
}
</style>
