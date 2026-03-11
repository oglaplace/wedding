<script setup>
import { ref, onMounted, nextTick } from 'vue'
import EnvelopeAnimation from './components/EnvelopeAnimation.vue'
import ConfettiEffect from './components/ConfettiEffect.vue'
import InvitationCard from './components/InvitationCard.vue'
import RsvpForm from './components/RsvpForm.vue'
import CountdownTimer from './components/CountdownTimer.vue'
import GoogleMap from './components/GoogleMap.vue'
import ShareButton from './components/ShareButton.vue'
import MusicPlayer from './components/MusicPlayer.vue'

const envelopeOpened = ref(false)
const showContent = ref(false)
const confettiRef = ref(null)
const musicPlayerRef = ref(null)

async function onEnvelopeOpened() {
  envelopeOpened.value = true
  await nextTick()
  setTimeout(() => {
    showContent.value = true
  }, 100)
  setTimeout(() => {
    confettiRef.value?.start()
    musicPlayerRef.value?.startMusic()
  }, 600)
}
</script>

<template>
  <div class="app-wrapper">
    <EnvelopeAnimation v-if="!envelopeOpened" @opened="onEnvelopeOpened" />

    <Transition name="fade">
      <div v-if="showContent" class="main-content">
        <MusicPlayer ref="musicPlayerRef" />
        <ConfettiEffect ref="confettiRef" />

        <!-- Hero / Invitation + RSVP -->
        <section class="section hero-section">
          <div class="hero-grid">
            <InvitationCard />
            <RsvpForm />
          </div>
        </section>

        <!-- Countdown -->
        <section class="section countdown-section">
          <CountdownTimer />
        </section>

        <!-- Map -->
        <section class="section map-section">
          <GoogleMap />
        </section>

        <!-- Footer -->
        <footer class="site-footer">
          <div class="footer-inner">
            <p class="footer-script">Grâce &amp; Robinson</p>
            <div class="gold-divider"></div>
            <p class="footer-families">Famille MBOUSSI &amp; Famille LOUSSAKOU</p>
            <ShareButton />
            <p class="footer-copy">© 2026 — Tous droits réservés</p>
          </div>
        </footer>
      </div>
    </Transition>
  </div>
</template>

<style scoped>
.app-wrapper {
  min-height: 100vh;
  background-color: var(--color-cream);
}

.main-content {
  min-height: 100vh;
}

/* Hero Section */
.hero-section {
  padding-top: 3rem;
  padding-bottom: 3rem;
}

.hero-grid {
  display: flex;
  flex-direction: row;
  gap: 2.5rem;
  align-items: flex-start;
  justify-content: center;
}

@media (max-width: 1024px) {
  .hero-grid {
    flex-direction: column;
    align-items: center;
  }
}

/* Countdown Section */
.countdown-section {
  background: linear-gradient(135deg, var(--color-brown-dark) 0%, var(--color-brown) 100%);
  max-width: 100%;
  padding: 4rem 1.5rem;
}

/* Map Section */
.map-section {
  padding: 4rem 1.5rem;
}

/* Footer */
.site-footer {
  background: linear-gradient(135deg, var(--color-brown-dark), #3a2510);
  color: var(--color-cream);
  text-align: center;
  padding: 3rem 1.5rem;
}

.footer-inner {
  max-width: 600px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1rem;
}

.footer-script {
  font-family: var(--font-script);
  font-size: 2.8rem;
  color: var(--color-gold);
  line-height: 1;
}

.footer-families {
  font-family: var(--font-serif);
  font-size: 0.95rem;
  color: var(--color-cream-dark);
  letter-spacing: 0.05em;
}

.footer-copy {
  font-size: 0.75rem;
  color: rgba(255, 248, 231, 0.5);
  margin-top: 0.5rem;
}
</style>
