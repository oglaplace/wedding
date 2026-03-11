<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const isPlaying = ref(false)
const showTooltip = ref(false)
const audioRef = ref(null)
let audioCtx = null
let gainNode = null
let oscillator = null
let useWebAudio = false

function createWebAudioTone() {
  try {
    audioCtx = new (window.AudioContext || window.webkitAudioContext)()
    gainNode = audioCtx.createGain()
    gainNode.gain.setValueAtTime(0, audioCtx.currentTime)
    gainNode.connect(audioCtx.destination)
    useWebAudio = true
  } catch {
    useWebAudio = false
  }
}

function startWebAudioTone() {
  if (!audioCtx) return
  if (oscillator) {
    try { oscillator.stop() } catch {}
  }
  oscillator = audioCtx.createOscillator()
  oscillator.type = 'sine'
  oscillator.frequency.setValueAtTime(220, audioCtx.currentTime)

  const osc2 = audioCtx.createOscillator()
  osc2.type = 'sine'
  osc2.frequency.setValueAtTime(330, audioCtx.currentTime)

  const osc3 = audioCtx.createOscillator()
  osc3.type = 'sine'
  osc3.frequency.setValueAtTime(440, audioCtx.currentTime)

  oscillator.connect(gainNode)
  osc2.connect(gainNode)
  osc3.connect(gainNode)

  oscillator.start()
  osc2.start()
  osc3.start()

  gainNode.gain.setValueAtTime(0, audioCtx.currentTime)
  gainNode.gain.linearRampToValueAtTime(0.04, audioCtx.currentTime + 0.5)
}

function stopWebAudioTone() {
  if (!gainNode || !audioCtx) return
  gainNode.gain.setValueAtTime(gainNode.gain.value, audioCtx.currentTime)
  gainNode.gain.linearRampToValueAtTime(0, audioCtx.currentTime + 0.5)
  setTimeout(() => {
    try { oscillator?.stop() } catch {}
  }, 600)
}

async function startMusic() {
  const audio = audioRef.value
  if (!audio) return

  try {
    audio.volume = 0.35
    await audio.play()
    isPlaying.value = true
    showTooltip.value = false
  } catch {
    // Autoplay blocked — show tooltip
    showTooltip.value = true
    isPlaying.value = false
  }
}

async function toggleMusic() {
  showTooltip.value = false
  const audio = audioRef.value

  if (isPlaying.value) {
    if (useWebAudio) {
      stopWebAudioTone()
    } else if (audio) {
      audio.pause()
    }
    isPlaying.value = false
  } else {
    if (audio && !useWebAudio) {
      try {
        audio.volume = 0.35
        await audio.play()
        isPlaying.value = true
      } catch {
        // Fallback to Web Audio
        if (!audioCtx) createWebAudioTone()
        startWebAudioTone()
        isPlaying.value = true
      }
    } else {
      if (!audioCtx) createWebAudioTone()
      startWebAudioTone()
      isPlaying.value = true
    }
  }
}

function handleAudioError() {
  useWebAudio = true
}

onMounted(() => {
  const audio = audioRef.value
  if (audio) {
    audio.addEventListener('error', handleAudioError)
  }
})

onUnmounted(() => {
  try { oscillator?.stop() } catch {}
  try { audioCtx?.close() } catch {}
})

defineExpose({ startMusic })
</script>

<template>
  <div class="music-player">
    <!-- Tooltip -->
    <Transition name="tooltip-fade">
      <div v-if="showTooltip" class="music-tooltip">
        Cliquez pour activer la musique 🎵
      </div>
    </Transition>

    <button
      class="music-btn"
      :class="{ playing: isPlaying }"
      @click="toggleMusic"
      :title="isPlaying ? 'Couper la musique' : 'Activer la musique'"
      type="button"
      aria-label="Contrôle de la musique"
    >
      <span class="music-icon">{{ isPlaying ? '🔊' : '🔇' }}</span>
      <div v-if="isPlaying" class="sound-waves">
        <span class="wave w1"></span>
        <span class="wave w2"></span>
        <span class="wave w3"></span>
      </div>
    </button>

    <audio
      ref="audioRef"
      src="/audio/background-music.mp3"
      loop
      preload="none"
    ></audio>
  </div>
</template>

<style scoped>
.music-player {
  position: fixed;
  top: 1.2rem;
  right: 1.2rem;
  z-index: 800;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 0.5rem;
}

/* ── Button ── */
.music-btn {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  background: linear-gradient(135deg, var(--color-brown-dark), #3e2710);
  border: 2px solid rgba(212, 175, 55, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  box-shadow: 0 4px 20px rgba(0,0,0,0.3);
  transition: var(--transition);
  position: relative;
  overflow: visible;
}

.music-btn:hover {
  transform: scale(1.08);
  border-color: var(--color-gold);
  box-shadow: 0 4px 24px rgba(212, 175, 55, 0.4);
}

.music-btn.playing {
  background: linear-gradient(135deg, var(--color-brown), var(--color-gold-light));
}

.music-icon {
  font-size: 1.2rem;
  line-height: 1;
}

/* ── Sound wave animation ── */
.sound-waves {
  position: absolute;
  right: -4px;
  bottom: -4px;
  display: flex;
  align-items: flex-end;
  gap: 2px;
}

.wave {
  display: block;
  width: 3px;
  border-radius: 2px;
  background: var(--color-gold);
  animation: wave-bounce 0.8s ease-in-out infinite;
}

.w1 { height: 6px; animation-delay: 0s; }
.w2 { height: 10px; animation-delay: 0.15s; }
.w3 { height: 7px; animation-delay: 0.3s; }

@keyframes wave-bounce {
  0%, 100% { transform: scaleY(0.5); }
  50% { transform: scaleY(1.5); }
}

/* ── Tooltip ── */
.music-tooltip {
  background: var(--color-brown-dark);
  color: var(--color-cream);
  padding: 0.5rem 1rem;
  border-radius: 6px;
  font-size: 0.78rem;
  font-family: var(--font-sans);
  white-space: nowrap;
  border: 1px solid rgba(212, 175, 55, 0.3);
  box-shadow: 0 4px 16px rgba(0,0,0,0.3);
  position: relative;
}

.music-tooltip::after {
  content: '';
  position: absolute;
  top: 50%;
  right: -6px;
  transform: translateY(-50%);
  border: 6px solid transparent;
  border-left-color: var(--color-brown-dark);
  border-right: none;
}

/* Tooltip transition */
.tooltip-fade-enter-active,
.tooltip-fade-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.tooltip-fade-enter-from,
.tooltip-fade-leave-to {
  opacity: 0;
  transform: translateX(10px);
}
</style>
