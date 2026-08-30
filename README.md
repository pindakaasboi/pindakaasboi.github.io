# 🍽️ Aan Tafel

**Aan Tafel** is een speelse, in het Nederlands geschreven webapp waarin je een virtueel bord met eten samenstelt en Gemini laat vertellen hoe gezond, seizoensgebonden en klimaatvriendelijk je maaltijd is. Daarnaast bevat de app een receptenkast met basisrecepten en een AI-receptengenerator, plus een (grotendeels fictieve) Premium-laag met accounts.

De hele app is één self-contained HTML-bestand (`index.html`) met inline CSS en JavaScript — geen build-stap, geen dependencies om te installeren.

---

## ✨ Features

### Mijn bord
- Sleep of tik ingrediënten uit de zijbalk op een bord.
- Tik op eten dat al op je bord ligt om het weer te verwijderen.
- Ken je een ingrediënt niet? Voeg het toe via **Gemini** — de AI bedenkt een categorie en stelt emoji voor.
- Klik op **"Analyseer mijn bord"** voor een AI-beoordeling van:
  - gezondheid (score + samenvatting)
  - seizoensgebondenheid (percentage + toelichting)
  - CO₂-impact (schatting per ingrediënt en totaal)
  - een gevarieerde, op je bord toegespitste tip (geen standaard "haal de kip weg"-advies)
- Upload een foto van je échte bord (PNG/JPG) en laat Gemini die herkennen en analyseren.
- **Desktop only:** een taartdiagram en statistiekjes over je bord in de zijbalk.

### Recepten
- 6 vaste basisrecepten, direct beschikbaar zonder API-sleutel.
- Laat Gemini een **vrij recept** verzinnen op basis van een wens en filters (vegetarisch, snel, weinig CO₂).
- Of laat Gemini iets bedenken **met wat je toevallig in huis hebt**.

### Weergave
- Handmatige 🖥️/📱 schakelaar in de header om te wisselen tussen desktop- en mobiele layout, los van je daadwerkelijke schermbreedte.

### Accounts & Premium
- Echte login/registratie via **Firebase Authentication**.
- Premium-gebruikers krijgen (in theorie): hun bord automatisch bewaard, een wekelijks AI-recept, een boodschappenlijst-widget en Gemini-toegang zonder eigen API-sleutel.
- De statusindicator naast de instellingenknop is **rood** (geen sleutel), **groen** (eigen werkende sleutel) of **goud** (Premium).

> ⚠️ **Let op — dit is een demo/hobbyproject, geen productie-app:**
> - Er zit **geen echte betaling** achter Premium. De "Upgrade"-knop toont alleen een bericht; activeren kan alleen via een hardcoded inwisselcode in de broncode.
> - Premium-gebruikers hebben **geen echte gedeelde Gemini-sleutel** — zonder eigen sleutel krijgen zij altijd de melding dat de gedeelde toegang nog geladen wordt, met het verzoek zelf een sleutel in te vullen.
> - Dit is bewust zo gebouwd; bouw je dit door naar een echt product, dan wil je een betaalprovider (Stripe/Mollie) en een eigen backend die veilig met een server-side Gemini-sleutel omgaat.

---

## 🧱 Tech stack

- Puro HTML/CSS/JS, geen framework en geen build-tool.
- [Google Fonts](https://fonts.google.com) (Fraunces, Inter, JetBrains Mono).
- [Firebase](https://firebase.google.com) (Auth + Firestore, via de compat-CDN-scripts) voor accounts en het bewaren van borden/voorkeuren.
- [Google Gemini API](https://ai.google.dev/) (`generateContent`, rechtstreeks vanuit de browser) voor alle AI-functionaliteit.

---

## 🚀 Aan de slag

### 1. Bestand openen
Omdat het één statisch HTML-bestand is, kun je het direct openen in de browser:

```bash
open index.html      # macOS
start index.html      # Windows
xdg-open index.html   # Linux
```

Voor functies die `fetch` gebruiken (Gemini, Firestore) werkt dit het prettigst via een lokale server in plaats van het `file://`-protocol:

```bash
npx serve .
# of
python3 -m http.server 8000
```

### 2. Je eigen Gemini API-sleutel
De app werkt volledig zonder eigen Firebase-project, zolang je je eigen Gemini-sleutel invult:

1. Haal een gratis sleutel op bij [Google AI Studio](https://aistudio.google.com/apikey).
2. Klik in de app op de sleutel-knop rechtsboven en plak je sleutel.
3. Kies een model uit de lijst die verschijnt.

Je sleutel wordt alleen in het geheugen van de sessie bewaard (of, als je bent ingelogd, in je eigen Firestore-document) — nooit gedeeld met anderen.

### 3. (Optioneel) je eigen Firebase-project
Wil je accounts en het bewaren van borden zelf laten werken in plaats van het meegeleverde demoproject te gebruiken?

1. Maak een project aan op de [Firebase Console](https://console.firebase.google.com/).
2. Zet **Authentication → Sign-in method → E-mail/wachtwoord** aan.
3. Maak een **Firestore-database** aan.
4. Vervang het `firebaseConfig`-object bovenin het `<script>`-blok onderaan `index.html` door de config van jouw eigen project.
5. Zet passende **Firestore-rules**, bijvoorbeeld: gebruikers mogen alleen hun eigen `users/{uid}`-document lezen/schrijven, en `config/app` is alleen leesbaar (voor de gedeelde Premium-Gemini-config, indien je die zelf wilt invullen).

---

## 📁 Projectstructuur

```
.
├── index.html   ← de volledige app: HTML, CSS en JS in één bestand
└── README.md
```

Alles staat bewust in één bestand zodat je het zonder build-stap kunt hosten, bijvoorbeeld via GitHub Pages, Netlify of Vercel (sleep het bestand er gewoon in).

---

## 🌍 Hosten op GitHub Pages

1. Push dit bestand naar een repository.
2. Ga naar **Settings → Pages**.
3. Kies als bron de branch/map waar `index.html` in staat.
4. Klaar — je app draait op `https://<gebruikersnaam>.github.io/<repo>/`.

---

## ⚠️ Disclaimer

Alle gezondheids-, seizoens- en CO₂-inschattingen zijn AI-gegenereerd ter indicatie en **geen** gecertificeerde voedings- of milieudata. Gebruik de app voor inspiratie, niet als medisch of wetenschappelijk advies.

---

## 📄 Licentie

Voeg hier je gewenste licentie toe (bijvoorbeeld MIT) als je dit project publiek wilt delen.
