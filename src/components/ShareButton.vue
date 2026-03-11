<script setup>
import { ref } from 'vue'

const toast = ref({ show: false, message: '' })

function showToast(message) {
  toast.value = { show: true, message }
  setTimeout(() => { toast.value.show = false }, 3000)
}

async function shareInvitation() {
  const shareData = {
    title: 'Grâce & Robinson — Mariage Coutumier',
    text: 'Vous êtes invité(e) au mariage coutumier de Grâce & Robinson le 30 Juillet 2026 à Brazzaville !',
    url: window.location.href,
  }

  try {
    if (navigator.share) {
      await navigator.share(shareData)
    } else {
      await navigator.clipboard.writeText(window.location.href)
      showToast('Lien copié ! 🎉')
    }
  } catch (err) {
    if (err.name !== 'AbortError') {
      // Fallback: manual copy
      try {
        await navigator.clipboard.writeText(window.location.href)
        showToast('Lien copié ! 🎉')
      } catch {
        showToast('Impossible de copier le lien.')
      }
    }
  }
}
</script>

<template>
  <div class="share-wrapper">
    <button class="share-btn" @click="shareInvitation" type="button">
      <span class="share-icon">📤</span>
      <span>Partager cette invitation</span>
    </button>

    <div class="share-toast" :class="{ show: toast.show }">
      {{ toast.message }}
    </div>
  </div>
</template>

<style scoped>
.share-wrapper {
  position: relative;
  display: flex;
  justify-content: center;
}

.share-btn {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  background: rgba(255, 255, 255, 0.08);
  border: 1.5px solid rgba(212, 175, 55, 0.4);
  color: var(--color-gold-light);
  padding: 0.7rem 1.8rem;
  border-radius: 50px;
  font-family: var(--font-serif);
  font-size: 0.9rem;
  font-weight: 600;
  letter-spacing: 0.05em;
  cursor: pointer;
  transition: var(--transition);
}

.share-btn:hover {
  background: rgba(212, 175, 55, 0.15);
  border-color: var(--color-gold);
  color: var(--color-gold);
  transform: translateY(-2px);
  box-shadow: 0 4px 20px rgba(212, 175, 55, 0.2);
}

.share-icon {
  font-size: 1.1rem;
}

/* ── Toast ── */
.share-toast {
  position: fixed;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%) translateY(100px);
  background: var(--color-brown-dark);
  color: var(--color-cream);
  padding: 0.9rem 2rem;
  border-radius: 50px;
  font-size: 0.9rem;
  font-weight: 500;
  border: 1px solid var(--color-gold-light);
  box-shadow: 0 8px 32px rgba(0,0,0,0.3);
  z-index: 9999;
  transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
  white-space: nowrap;
}

.share-toast.show {
  transform: translateX(-50%) translateY(0);
}
</style>
