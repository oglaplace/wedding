<script setup>
import { ref, reactive } from 'vue'
import { WEDDING_CONFIG } from '../config/index.js'

const form = reactive({
  name: '',
  email: '',
  phone: '',
  guests: 1,
  attending: 'yes',
  notes: '',
})

const submitting = ref(false)
const toast = ref({ show: false, message: '', type: '' })

function showToast(message, type = 'success') {
  toast.value = { show: true, message, type }
  setTimeout(() => { toast.value.show = false }, 4000)
}

async function handleSubmit() {
  submitting.value = true
  try {
    const payload = {
      timestamp: new Date().toISOString(),
      name: form.name,
      email: form.email,
      phone: form.phone,
      guests: form.guests,
      attending: form.attending === 'yes' ? 'Présent(e)' : 'Absent(e)',
      notes: form.notes || '—',
    }

    if (WEDDING_CONFIG.googleSheetsUrl === 'YOUR_GOOGLE_SHEETS_SCRIPT_URL') {
      // Demo mode: simulate success
      await new Promise((r) => setTimeout(r, 800))
      showToast('Votre présence a été confirmée ! Merci. 🎉', 'success')
    } else {
      const res = await fetch(WEDDING_CONFIG.googleSheetsUrl, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(payload),
      })
      if (!res.ok) throw new Error('Network error')
      showToast('Votre présence a été confirmée ! Merci. 🎉', 'success')
    }

    // Reset form
    Object.assign(form, { name: '', email: '', phone: '', guests: 1, attending: 'yes', notes: '' })
  } catch {
    showToast('Une erreur s\'est produite. Veuillez réessayer.', 'error')
  } finally {
    submitting.value = false
  }
}
</script>

<template>
  <div class="rsvp-wrapper">
    <div class="rsvp-card">
      <!-- Header -->
      <div class="rsvp-header">
        <div class="header-ornament">✦</div>
        <h2 class="rsvp-title">Confirmer votre présence</h2>
        <p class="rsvp-subtitle">Merci de nous confirmer votre venue avant le 15 juillet 2026</p>
        <div class="gold-divider"></div>
      </div>

      <form class="rsvp-form" @submit.prevent="handleSubmit" novalidate>
        <!-- Nom -->
        <div class="field">
          <label for="rsvp-name" class="field-label">Nom complet <span class="required">*</span></label>
          <input
            id="rsvp-name"
            v-model="form.name"
            type="text"
            class="field-input"
            placeholder="Votre nom et prénom"
            required
            :disabled="submitting"
          />
        </div>

        <!-- Email -->
        <div class="field">
          <label for="rsvp-email" class="field-label">Adresse e-mail <span class="required">*</span></label>
          <input
            id="rsvp-email"
            v-model="form.email"
            type="email"
            class="field-input"
            placeholder="votre@email.com"
            required
            :disabled="submitting"
          />
        </div>

        <!-- Téléphone -->
        <div class="field">
          <label for="rsvp-phone" class="field-label">Numéro de téléphone <span class="required">*</span></label>
          <input
            id="rsvp-phone"
            v-model="form.phone"
            type="tel"
            class="field-input"
            placeholder="+242 06 000 0000"
            required
            :disabled="submitting"
          />
        </div>

        <!-- Nombre d'invités -->
        <div class="field">
          <label for="rsvp-guests" class="field-label">Nombre d'invités <span class="required">*</span></label>
          <input
            id="rsvp-guests"
            v-model.number="form.guests"
            type="number"
            class="field-input"
            min="1"
            max="10"
            required
            :disabled="submitting"
          />
        </div>

        <!-- Confirmation de présence -->
        <div class="field">
          <fieldset class="radio-group" :disabled="submitting">
            <legend class="field-label">Confirmation de présence <span class="required">*</span></legend>
            <label class="radio-label" :class="{ selected: form.attending === 'yes' }">
              <input type="radio" v-model="form.attending" value="yes" class="radio-input" />
              <span class="radio-box">
                <span class="radio-check">✓</span>
              </span>
              <span>Je serai présent(e)</span>
            </label>
            <label class="radio-label" :class="{ selected: form.attending === 'no' }">
              <input type="radio" v-model="form.attending" value="no" class="radio-input" />
              <span class="radio-box">
                <span class="radio-check">✓</span>
              </span>
              <span>Je ne pourrai pas être présent(e)</span>
            </label>
          </fieldset>
        </div>

        <!-- Notes -->
        <div class="field">
          <label for="rsvp-notes" class="field-label">Restrictions alimentaires ou notes <span class="optional">(optionnel)</span></label>
          <textarea
            id="rsvp-notes"
            v-model="form.notes"
            class="field-input field-textarea"
            placeholder="Allergies, régime alimentaire, message pour les mariés…"
            rows="3"
            :disabled="submitting"
          ></textarea>
        </div>

        <!-- Submit -->
        <button type="submit" class="submit-btn" :disabled="submitting">
          <span v-if="!submitting">Confirmer ma présence ✦</span>
          <span v-else class="loading-dots">Envoi en cours<span>.</span><span>.</span><span>.</span></span>
        </button>
      </form>
    </div>

    <!-- Toast notification -->
    <div
      class="form-toast"
      :class="[{ show: toast.show }, toast.type]"
    >
      {{ toast.message }}
    </div>
  </div>
</template>

<style scoped>
.rsvp-wrapper {
  width: 100%;
  max-width: 480px;
  position: relative;
  flex-shrink: 0;
}

.rsvp-card {
  background: var(--color-cream);
  border-radius: 8px;
  box-shadow: 0 8px 40px rgba(92, 61, 30, 0.2), 0 2px 8px rgba(0,0,0,0.08);
  overflow: hidden;
  border: 1px solid rgba(212, 175, 55, 0.25);
}

/* ── Header ── */
.rsvp-header {
  background: linear-gradient(135deg, var(--color-brown-dark) 0%, #3e2710 100%);
  padding: 2rem 2rem 1.5rem;
  text-align: center;
  color: var(--color-cream);
}

.header-ornament {
  color: var(--color-gold);
  font-size: 1.2rem;
  margin-bottom: 0.5rem;
}

.rsvp-title {
  font-family: var(--font-serif);
  font-size: 1.5rem;
  font-weight: 600;
  color: var(--color-cream);
  margin-bottom: 0.4rem;
}

.rsvp-subtitle {
  font-size: 0.8rem;
  opacity: 0.7;
  font-style: italic;
  margin-bottom: 0.8rem;
}

/* ── Form ── */
.rsvp-form {
  padding: 1.6rem 2rem 2rem;
  display: flex;
  flex-direction: column;
  gap: 1.2rem;
}

.field {
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
}

.field-label {
  font-size: 0.8rem;
  font-weight: 600;
  color: var(--color-brown-dark);
  text-transform: uppercase;
  letter-spacing: 0.06em;
}

.required {
  color: var(--color-brown);
}

.optional {
  font-weight: 400;
  text-transform: none;
  color: var(--color-brown);
  opacity: 0.7;
  font-size: 0.75rem;
  letter-spacing: 0;
}

.field-input {
  padding: 0.7rem 1rem;
  border: 1.5px solid rgba(212, 175, 55, 0.4);
  border-radius: 4px;
  background: white;
  font-size: 0.9rem;
  color: var(--color-brown-dark);
  transition: var(--transition);
  outline: none;
}

.field-input:focus {
  border-color: var(--color-gold);
  box-shadow: 0 0 0 3px rgba(212, 175, 55, 0.15);
}

.field-input:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.field-textarea {
  resize: vertical;
  min-height: 80px;
  line-height: 1.5;
}

/* ── Radio buttons ── */
.radio-group {
  border: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 0.6rem;
}

.radio-group > legend {
  margin-bottom: 0.5rem;
  width: 100%;
}

.radio-label {
  display: flex;
  align-items: center;
  gap: 0.6rem;
  cursor: pointer;
  font-size: 0.88rem;
  color: var(--color-brown-dark);
  padding: 0.5rem 0.8rem;
  border-radius: 4px;
  border: 1.5px solid rgba(212, 175, 55, 0.3);
  background: white;
  transition: var(--transition);
}

.radio-label:hover {
  border-color: var(--color-gold-light);
  background: rgba(212, 175, 55, 0.05);
}

.radio-label.selected {
  border-color: var(--color-gold);
  background: rgba(212, 175, 55, 0.08);
}

.radio-input {
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
}

.radio-box {
  width: 18px;
  height: 18px;
  border-radius: 50%;
  border: 2px solid var(--color-gold-light);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  transition: var(--transition);
}

.radio-label.selected .radio-box {
  background: var(--color-gold);
  border-color: var(--color-gold);
}

.radio-check {
  font-size: 0.65rem;
  color: white;
  opacity: 0;
  transition: opacity 0.2s;
}

.radio-label.selected .radio-check {
  opacity: 1;
}

/* ── Submit button ── */
.submit-btn {
  background: linear-gradient(135deg, var(--color-brown-dark), #3e2710);
  color: var(--color-cream);
  padding: 0.9rem 2rem;
  border-radius: 50px;
  font-family: var(--font-serif);
  font-size: 1rem;
  font-weight: 600;
  letter-spacing: 0.05em;
  border: 2px solid transparent;
  transition: var(--transition);
  box-shadow: var(--shadow-gold);
  position: relative;
  overflow: hidden;
}

.submit-btn::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, var(--color-gold), var(--color-gold-light));
  opacity: 0;
  transition: opacity 0.3s;
}

.submit-btn:hover::before {
  opacity: 1;
}

.submit-btn:hover {
  color: var(--color-brown-dark);
  border-color: var(--color-gold);
  transform: translateY(-1px);
}

.submit-btn:disabled {
  opacity: 0.7;
  cursor: not-allowed;
  transform: none;
}

.submit-btn span {
  position: relative;
  z-index: 1;
}

/* Loading dots animation */
.loading-dots span {
  animation: blink 1.2s infinite;
}
.loading-dots span:nth-child(2) { animation-delay: 0.2s; }
.loading-dots span:nth-child(3) { animation-delay: 0.4s; }

@keyframes blink {
  0%, 80%, 100% { opacity: 0; }
  40% { opacity: 1; }
}

/* ── Toast ── */
.form-toast {
  position: fixed;
  bottom: 2rem;
  left: 50%;
  transform: translateX(-50%) translateY(100px);
  padding: 1rem 2rem;
  border-radius: 50px;
  font-size: 0.9rem;
  font-weight: 500;
  z-index: 9999;
  transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
  white-space: nowrap;
  max-width: 90vw;
  text-align: center;
}

.form-toast.success {
  background: var(--color-brown-dark);
  color: var(--color-cream);
  border: 1px solid var(--color-gold-light);
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
}

.form-toast.error {
  background: #7a2020;
  color: #ffe0e0;
  border: 1px solid #c04040;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
}

.form-toast.show {
  transform: translateX(-50%) translateY(0);
}

/* ── Responsive ── */
@media (max-width: 480px) {
  .rsvp-form {
    padding: 1.2rem 1.2rem 1.6rem;
  }

  .rsvp-header {
    padding: 1.5rem 1.2rem 1rem;
  }

  .rsvp-title {
    font-size: 1.3rem;
  }

  .rsvp-wrapper {
    max-width: 100%;
  }
}
</style>
