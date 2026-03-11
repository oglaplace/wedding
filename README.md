# 💍 Grâce & Robinson — Site d'invitation de mariage coutumier (DOT)

> **French / Français** | [English below](#english-version)

Un site web d'invitation de mariage entièrement en Vue.js 3, créé pour le mariage coutumier (DOT) de Grâce & Robinson le 30 juillet 2026 à Brazzaville, Congo.

---

## 🇫🇷 Version Française

### Fonctionnalités

- 🎉 **Animation d'enveloppe** — Une enveloppe dorée s'ouvre avant de révéler l'invitation
- 🎨 **Carton d'invitation HTML/CSS** — Reproduction fidèle du carton imprimé
- 📋 **Formulaire RSVP** — Envoi vers Google Sheets via Apps Script
- ⏱️ **Compte à rebours** — Décompte en temps réel jusqu'au 30 juillet 2026
- 🗺️ **Carte Google Maps** — Intégrée avec lien vers l'itinéraire
- 🎵 **Lecteur de musique** — Musique de fond avec fallback Web Audio API
- 🎊 **Confettis dorés** — Animation canvas au chargement
- 📤 **Bouton de partage** — Web Share API ou copie du lien

### Prérequis

- Node.js 18+ et npm

### Installation et développement

```bash
# Cloner le dépôt
git clone https://github.com/VOTRE_UTILISATEUR/wedding.git
cd wedding

# Installer les dépendances
npm install

# Démarrer le serveur de développement
npm run dev
```

### Construction pour la production

```bash
npm run build
```

Les fichiers générés se trouvent dans le répertoire `dist/`.

### Déploiement sur GitHub Pages

1. Assurez-vous que `vite.config.js` contient `base: '/wedding/'`
2. Créez le workflow GitHub Actions `.github/workflows/deploy.yml` :

```yaml
name: Deploy to GitHub Pages
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm install
      - run: npm run build
      - uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

3. Activez GitHub Pages dans les paramètres du dépôt (source : branche `gh-pages`)

### Configuration de Google Sheets (formulaire RSVP)

1. Ouvrez Google Sheets et créez une feuille avec les colonnes :
   `Timestamp | Nom | Email | Téléphone | Invités | Présence | Notes`

2. Dans Google Sheets, allez dans **Extensions > Apps Script** et collez ce code :

```javascript
function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const data = JSON.parse(e.postData.contents);
  sheet.appendRow([
    data.timestamp,
    data.name,
    data.email,
    data.phone,
    data.guests,
    data.attending,
    data.notes
  ]);
  return ContentService
    .createTextOutput(JSON.stringify({ result: 'success' }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

3. Cliquez sur **Déployer > Nouveau déploiement** :
   - Type : Application web
   - Exécuter en tant que : Moi
   - Qui a accès : Tout le monde

4. Copiez l'URL du déploiement et remplacez `YOUR_GOOGLE_SHEETS_SCRIPT_URL` dans `src/config/index.js`

### Musique de fond

1. Placez votre fichier audio dans `public/audio/background-music.mp3`
2. En l'absence de fichier MP3, l'application génère une tonalité douce via la Web Audio API
3. Formats supportés : MP3, OGG, WAV

### Personnalisation

Toutes les données du mariage se trouvent dans `src/config/index.js` :

```js
export const WEDDING_CONFIG = {
  couple: { bride: 'Grâce', groom: 'Robinson' },
  families: { bride: 'MBOUSSI', groom: 'LOUSSAKOU' },
  date: new Date('2026-07-30T10:00:00+01:00'),
  address: { ... },
  googleSheetsUrl: 'YOUR_URL',
}
```

**Palette de couleurs** (dans `src/style.css`) :
| Variable | Valeur | Usage |
|---|---|---|
| `--color-gold` | `#D4AF37` | Couleur principale dorée |
| `--color-gold-light` | `#C8A850` | Dorée claire |
| `--color-brown-dark` | `#5C3D1E` | Brun foncé |
| `--color-brown` | `#8B6914` | Brun moyen |
| `--color-cream` | `#FFF8E7` | Crème clair |
| `--color-cream-dark` | `#FAF0DB` | Crème foncé |

---

## English Version

A complete Vue.js 3 wedding invitation website for Grâce & Robinson's customary wedding (DOT) on July 30, 2026 in Brazzaville, Congo.

### Features

- 🎉 **Envelope animation** — A golden envelope splits open to reveal the invitation
- 🎨 **HTML/CSS invitation card** — Faithful reproduction of the printed card
- 📋 **RSVP form** — Submits to Google Sheets via Apps Script
- ⏱️ **Countdown timer** — Live countdown to July 30, 2026
- 🗺️ **Google Maps embed** — With directions link
- 🎵 **Music player** — Background music with Web Audio API fallback
- 🎊 **Gold confetti** — Canvas animation on load
- 📤 **Share button** — Web Share API or clipboard copy

### Prerequisites

- Node.js 18+ and npm

### Installation & Development

```bash
git clone https://github.com/YOUR_USERNAME/wedding.git
cd wedding
npm install
npm run dev
```

### Production Build

```bash
npm run build
```

### Google Sheets Setup (RSVP Form)

1. Create a Google Sheet with columns: `Timestamp | Name | Email | Phone | Guests | Attending | Notes`
2. Go to **Extensions > Apps Script** and paste the Apps Script code shown in the French section above
3. Deploy as a web app (accessible by anyone)
4. Copy the deployment URL and update `googleSheetsUrl` in `src/config/index.js`

### Background Music

Place your audio file at `public/audio/background-music.mp3`. If not present, the app generates a gentle Web Audio API tone as a fallback.

### GitHub Pages Deployment

Set `base: '/wedding/'` in `vite.config.js` (already done), then use the GitHub Actions workflow shown in the French section above.

---

## Structure du projet / Project Structure

```
wedding/
├── public/
│   └── audio/
│       └── background-music.mp3   ← Ajoutez votre musique ici
├── src/
│   ├── components/
│   │   ├── ConfettiEffect.vue
│   │   ├── CountdownTimer.vue
│   │   ├── EnvelopeAnimation.vue
│   │   ├── GoogleMap.vue
│   │   ├── InvitationCard.vue
│   │   ├── MusicPlayer.vue
│   │   ├── RsvpForm.vue
│   │   └── ShareButton.vue
│   ├── config/
│   │   └── index.js               ← Configuration centralisée
│   ├── App.vue
│   ├── main.js
│   └── style.css
├── index.html
├── package.json
└── vite.config.js
```

---

*Fait avec ❤️ pour Grâce & Robinson — Mariage Coutumier DOT, 30 Juillet 2026, Brazzaville*
