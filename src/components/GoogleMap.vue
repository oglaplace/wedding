<script setup>
import { WEDDING_CONFIG } from '../config/index.js'

const mapSrc = `https://maps.google.com/maps?q=${encodeURIComponent(WEDDING_CONFIG.address.mapQuery)}&output=embed`
</script>

<template>
  <div class="map-wrapper">
    <!-- Section title -->
    <div class="map-header">
      <p class="map-script">Nous vous attendons</p>
      <h2 class="map-title">Lieu de la cérémonie</h2>
      <div class="gold-divider"></div>
    </div>

    <!-- Address card -->
    <div class="address-card">
      <div class="address-icon">📍</div>
      <div class="address-details">
        <p class="address-name">{{ WEDDING_CONFIG.address.street }}</p>
        <p class="address-line">{{ WEDDING_CONFIG.address.area }} — {{ WEDDING_CONFIG.address.reference }}</p>
        <p class="address-city">{{ WEDDING_CONFIG.address.city }}</p>
      </div>
      <a
        :href="`https://www.google.com/maps/search/?api=1&query=${encodeURIComponent(WEDDING_CONFIG.address.mapQuery)}`"
        target="_blank"
        rel="noopener noreferrer"
        class="directions-btn"
      >
        Itinéraire →
      </a>
    </div>

    <!-- Map iframe -->
    <div class="map-container">
      <iframe
        :src="mapSrc"
        title="Lieu de la cérémonie — Mfilou, Brazzaville"
        loading="lazy"
        referrerpolicy="no-referrer-when-downgrade"
        allowfullscreen
        class="map-iframe"
      ></iframe>
    </div>
  </div>
</template>

<style scoped>
.map-wrapper {
  max-width: 900px;
  margin: 0 auto;
}

/* ── Header ── */
.map-header {
  text-align: center;
  margin-bottom: 2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.4rem;
}

.map-script {
  font-family: var(--font-script);
  font-size: 2rem;
  color: var(--color-brown);
  line-height: 1.1;
}

.map-title {
  font-family: var(--font-serif);
  font-size: 1.8rem;
  color: var(--color-brown-dark);
  font-weight: 700;
}

/* ── Address card ── */
.address-card {
  background: var(--color-cream-dark);
  border: 1px solid rgba(212, 175, 55, 0.3);
  border-radius: 8px;
  padding: 1.2rem 1.6rem;
  margin-bottom: 1.2rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  box-shadow: var(--shadow-brown);
  flex-wrap: wrap;
}

.address-icon {
  font-size: 1.8rem;
  flex-shrink: 0;
}

.address-details {
  flex: 1;
  min-width: 200px;
}

.address-name {
  font-family: var(--font-serif);
  font-weight: 600;
  font-size: 1rem;
  color: var(--color-brown-dark);
}

.address-line {
  font-size: 0.85rem;
  color: var(--color-brown);
  margin-top: 0.15rem;
}

.address-city {
  font-size: 0.85rem;
  color: var(--color-brown);
  font-style: italic;
}

.directions-btn {
  display: inline-block;
  background: linear-gradient(135deg, var(--color-brown-dark), #3e2710);
  color: var(--color-cream);
  padding: 0.6rem 1.4rem;
  border-radius: 50px;
  font-size: 0.85rem;
  font-weight: 600;
  font-family: var(--font-sans);
  text-decoration: none;
  transition: var(--transition);
  white-space: nowrap;
  border: 1px solid rgba(212, 175, 55, 0.3);
}

.directions-btn:hover {
  background: linear-gradient(135deg, var(--color-gold), var(--color-gold-light));
  color: var(--color-brown-dark);
  transform: translateY(-1px);
  box-shadow: var(--shadow-gold);
}

/* ── Map ── */
.map-container {
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 8px 40px rgba(92, 61, 30, 0.2);
  border: 2px solid rgba(212, 175, 55, 0.25);
  line-height: 0;
}

.map-iframe {
  width: 100%;
  height: 420px;
  border: none;
  display: block;
}

/* ── Responsive ── */
@media (max-width: 600px) {
  .map-iframe {
    height: 280px;
  }

  .address-card {
    flex-direction: column;
    align-items: flex-start;
    gap: 0.6rem;
  }

  .directions-btn {
    align-self: flex-start;
  }
}
</style>
