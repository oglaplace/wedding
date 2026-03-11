<script setup>
import { WEDDING_CONFIG } from '../config/index.js'
</script>

<template>
  <article class="invitation-card">
    <!-- Left brown panel -->
    <div class="card-left-panel">
      <div class="panel-swirls">
        <svg class="swirl-svg" viewBox="0 0 120 600" xmlns="http://www.w3.org/2000/svg">
          <path d="M60 0 Q90 50 60 100 Q30 150 60 200 Q90 250 60 300 Q30 350 60 400 Q90 450 60 500 Q30 550 60 600"
            fill="none" stroke="rgba(212,175,55,0.25)" stroke-width="2"/>
          <path d="M40 0 Q70 60 40 120 Q10 180 40 240 Q70 300 40 360 Q10 420 40 480 Q70 540 40 600"
            fill="none" stroke="rgba(212,175,55,0.15)" stroke-width="1.5"/>
          <path d="M80 0 Q50 70 80 140 Q110 210 80 280 Q50 350 80 420 Q110 490 80 560"
            fill="none" stroke="rgba(212,175,55,0.12)" stroke-width="1"/>
          <!-- Flower ornaments -->
          <circle cx="60" cy="150" r="8" fill="none" stroke="rgba(212,175,55,0.3)" stroke-width="1.5"/>
          <circle cx="60" cy="150" r="4" fill="rgba(212,175,55,0.2)"/>
          <circle cx="60" cy="300" r="10" fill="none" stroke="rgba(212,175,55,0.3)" stroke-width="1.5"/>
          <circle cx="60" cy="300" r="5" fill="rgba(212,175,55,0.2)"/>
          <circle cx="60" cy="450" r="8" fill="none" stroke="rgba(212,175,55,0.3)" stroke-width="1.5"/>
          <circle cx="60" cy="450" r="4" fill="rgba(212,175,55,0.2)"/>
        </svg>
      </div>
      <div class="dot-badge">DOT</div>
      <div class="panel-label">Mariage<br/>Coutumier</div>
    </div>

    <!-- Right main content -->
    <div class="card-main">
      <!-- Mandala ornament -->
      <div class="mandala">
        <div class="mandala-ring ring-1"></div>
        <div class="mandala-ring ring-2"></div>
        <div class="mandala-ring ring-3"></div>
        <div class="mandala-center">✦</div>
        <div class="mandala-rays">
          <div class="ray" v-for="i in 12" :key="i" :style="{ transform: `rotate(${i * 30}deg)` }"></div>
        </div>
      </div>

      <!-- Tagline -->
      <p class="tagline">{{ WEDDING_CONFIG.event.tagline }}</p>

      <!-- Couple names -->
      <h1 class="couple-names">
        {{ WEDDING_CONFIG.couple.bride }} &amp; {{ WEDDING_CONFIG.couple.groom }}
      </h1>

      <p class="invite-text">vous invitent à leur mariage coutumier</p>

      <div class="gold-divider"></div>

      <!-- Event details -->
      <div class="detail-boxes">
        <div class="detail-box dark">
          <span class="detail-icon">📅</span>
          <div>
            <span class="detail-label">Date</span>
            <span class="detail-value">{{ WEDDING_CONFIG.dateDisplay }}</span>
          </div>
        </div>
        <div class="detail-box dark">
          <span class="detail-icon">🕙</span>
          <div>
            <span class="detail-label">Heure</span>
            <span class="detail-value">{{ WEDDING_CONFIG.time }}</span>
          </div>
        </div>
        <div class="detail-box light">
          <span class="detail-icon">📍</span>
          <div>
            <span class="detail-label">Adresse</span>
            <span class="detail-value address-value">
              {{ WEDDING_CONFIG.address.street }},<br/>
              {{ WEDDING_CONFIG.address.area }}<br/>
              {{ WEDDING_CONFIG.address.reference }}<br/>
              {{ WEDDING_CONFIG.address.city }}
            </span>
          </div>
        </div>
      </div>

      <!-- Families -->
      <p class="families-script">
        Famille {{ WEDDING_CONFIG.families.bride }} &amp; Famille {{ WEDDING_CONFIG.families.groom }}
      </p>
    </div>
  </article>

  <!-- Image fallback -->
  <div class="image-fallback">
    <p class="fallback-label">Carton d'invitation original</p>
    <img
      :src="WEDDING_CONFIG.invitationImageFallback"
      alt="Carton d'invitation — Grâce & Robinson DOT"
      class="fallback-img"
      loading="lazy"
    />
  </div>
</template>

<style scoped>
/* ── Card wrapper ── */
.invitation-card {
  display: flex;
  flex-direction: row;
  width: 100%;
  max-width: 560px;
  min-height: 680px;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 16px 60px rgba(92, 61, 30, 0.35), 0 4px 20px rgba(0,0,0,0.15);
  background: var(--color-cream);
  flex-shrink: 0;
}

/* ── Left panel ── */
.card-left-panel {
  width: 80px;
  background: linear-gradient(180deg, #5C3D1E 0%, #3e2710 50%, #5C3D1E 100%);
  position: relative;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  padding: 1.2rem 0;
  border-right: 2px solid rgba(212, 175, 55, 0.3);
  flex-shrink: 0;
}

.panel-swirls {
  position: absolute;
  inset: 0;
  overflow: hidden;
}

.swirl-svg {
  width: 100%;
  height: 100%;
}

.dot-badge {
  position: relative;
  z-index: 1;
  font-family: var(--font-serif);
  font-weight: 700;
  font-size: 1.4rem;
  color: var(--color-gold);
  letter-spacing: 0.1em;
  text-shadow: 0 1px 6px rgba(0,0,0,0.4);
  writing-mode: vertical-rl;
  text-orientation: mixed;
  transform: rotate(180deg);
}

.panel-label {
  position: relative;
  z-index: 1;
  font-family: var(--font-serif);
  font-size: 0.6rem;
  color: rgba(255, 248, 231, 0.6);
  text-transform: uppercase;
  letter-spacing: 0.12em;
  text-align: center;
  writing-mode: vertical-rl;
  transform: rotate(180deg);
  line-height: 1.8;
}

/* ── Main content ── */
.card-main {
  flex: 1;
  padding: 1.8rem 1.6rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.8rem;
  background: linear-gradient(180deg, var(--color-cream) 0%, var(--color-cream-dark) 100%);
}

/* ── Mandala ornament ── */
.mandala {
  position: relative;
  width: 90px;
  height: 90px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.mandala-ring {
  position: absolute;
  border-radius: 50%;
  border: 1px solid var(--color-gold-light);
}

.ring-1 { width: 90px; height: 90px; opacity: 0.5; }
.ring-2 { width: 68px; height: 68px; opacity: 0.4; }
.ring-3 { width: 48px; height: 48px; background: rgba(212,175,55,0.08); }

.mandala-center {
  position: relative;
  z-index: 2;
  color: var(--color-gold);
  font-size: 1.2rem;
}

.mandala-rays {
  position: absolute;
  inset: 0;
}

.ray {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 1px;
  height: 44px;
  background: linear-gradient(transparent, var(--color-gold-light));
  transform-origin: top center;
  opacity: 0.35;
  margin-left: -0.5px;
  margin-top: 1px;
}

/* ── Tagline ── */
.tagline {
  font-family: var(--font-serif);
  font-style: italic;
  font-size: 0.72rem;
  color: var(--color-brown);
  text-align: center;
  line-height: 1.6;
  max-width: 360px;
  opacity: 0.85;
}

/* ── Names ── */
.couple-names {
  font-family: var(--font-script);
  font-size: 3rem;
  color: var(--color-brown-dark);
  text-align: center;
  line-height: 1.1;
  font-weight: 400;
}

.invite-text {
  font-family: var(--font-serif);
  font-size: 0.85rem;
  color: var(--color-brown);
  text-align: center;
  letter-spacing: 0.03em;
}

/* ── Detail boxes ── */
.detail-boxes {
  width: 100%;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.detail-box {
  display: flex;
  align-items: flex-start;
  gap: 0.75rem;
  padding: 0.65rem 0.9rem;
  border-radius: 4px;
}

.detail-box.dark {
  background: var(--color-brown-dark);
  color: var(--color-cream);
}

.detail-box.light {
  background: var(--color-cream-dark);
  color: var(--color-brown-dark);
  border: 1px solid rgba(212, 175, 55, 0.3);
}

.detail-icon {
  font-size: 1rem;
  flex-shrink: 0;
  margin-top: 2px;
}

.detail-box > div {
  display: flex;
  flex-direction: column;
  gap: 0.1rem;
}

.detail-label {
  font-size: 0.65rem;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  opacity: 0.7;
  font-weight: 600;
}

.detail-value {
  font-family: var(--font-serif);
  font-size: 0.85rem;
  font-weight: 600;
  line-height: 1.4;
}

.detail-box.dark .detail-value { color: var(--color-gold-light); }
.detail-box.light .detail-value { color: var(--color-brown-dark); }

.address-value {
  font-size: 0.78rem;
  font-weight: 500;
}

/* ── Families ── */
.families-script {
  font-family: var(--font-script);
  font-size: 1.4rem;
  color: var(--color-brown);
  text-align: center;
  margin-top: auto;
}

/* ── Image fallback ── */
.image-fallback {
  max-width: 560px;
  width: 100%;
  margin-top: 1.5rem;
  text-align: center;
}

.fallback-label {
  font-family: var(--font-serif);
  font-size: 0.8rem;
  color: var(--color-brown);
  font-style: italic;
  margin-bottom: 0.6rem;
  opacity: 0.7;
}

.fallback-img {
  width: 100%;
  border-radius: 6px;
  box-shadow: 0 8px 32px rgba(92, 61, 30, 0.25);
  border: 1px solid rgba(212, 175, 55, 0.3);
}

/* ── Responsive ── */
@media (max-width: 480px) {
  .invitation-card {
    min-height: auto;
    max-width: 100%;
  }

  .card-left-panel {
    width: 60px;
  }

  .dot-badge {
    font-size: 1.1rem;
  }

  .couple-names {
    font-size: 2.4rem;
  }

  .card-main {
    padding: 1.2rem 1rem;
    gap: 0.6rem;
  }
}
</style>
