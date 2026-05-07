# Poengliste 17. mai

Sanntids-poengliste for gutta. Single-file HTML + Firebase + PWA. Legg på hjemskjermen i Safari.

## Hva du trenger

- Gratis Google-konto (for Firebase)
- Gratis GitHub-konto (for hosting)
- 10 minutter

---

## Steg 1 — Lag Firebase-prosjekt

1. Gå til https://console.firebase.google.com og logg inn.
2. Klikk **"Add project"** / **"Legg til prosjekt"**.
3. Kall prosjektet `poengliste-17mai` (eller hva du vil). Klikk videre.
4. Slå **AV** Google Analytics. Klikk **"Create project"**.
5. Vent til prosjektet er ferdig. Klikk **"Continue"**.

## Steg 2 — Legg til en web-app

1. På prosjekt-forsiden klikker du på **`</>`-ikonet** (Web).
2. Skriv et kallenavn, f.eks. `poengliste-web`. **Ikke** huk av Hosting. Klikk **"Register app"**.
3. Du får nå en `firebaseConfig`-blokk. **Kopier hele blokken** (alt mellom `const firebaseConfig = {` og `};`).
4. Klikk **"Continue to console"**.

## Steg 3 — Slå på Firestore

1. I venstre meny: **"Build" → "Firestore Database"**.
2. Klikk **"Create database"**.
3. Velg lokasjon **`eur3 (europe-west)`**. Klikk **"Next"**.
4. Velg **"Start in test mode"**. Klikk **"Create"**.
5. Når basen er klar, klikk fanen **"Rules"** og lim inn:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /poengliste-2026/{doc} {
      allow read, write: if true;
    }
  }
}
```

6. Klikk **"Publish"**.

## Steg 4 — Lim inn config i appen

Åpne `index.html` i editoren din. Finn denne biten øverst i `<script>`-delen:

```js
const firebaseConfig = {
  apiKey: "PASTE_HER",
  ...
};
```

Bytt ut hele blokken med din egen `firebaseConfig` fra Steg 2.

---

## Steg 5 — Test lokalt

Kjør én linje av gangen i Terminal:

```
cd ~/poengliste-17mai
```

```
python3 -m http.server 8080
```

Åpne http://localhost:8080 i nettleseren. Lag en spiller, trykk noen pluss-knapper. Hvis du ser tallene oppdatere seg → Firebase funker.

Stopp serveren med `Ctrl+C` når du er ferdig.

---

## Steg 6 — Push til GitHub

Lag først et nytt **public** repo på https://github.com/new (kall det `poengliste-17mai`, ikke huk av README/license).

Så kjør disse, én linje av gangen:

```
cd ~/poengliste-17mai
```

```
git init -b main
```

```
git add .
```

```
git commit -m "init poengliste"
```

Bytt `DITT-BRUKERNAVN` med ditt GitHub-brukernavn:

```
git remote add origin https://github.com/DITT-BRUKERNAVN/poengliste-17mai.git
```

```
git push -u origin main
```

## Steg 7 — Slå på GitHub Pages

1. Gå til repoet ditt på github.com.
2. **Settings → Pages** (i venstre meny).
3. Under **"Source"**: velg **"Deploy from a branch"**.
4. Branch: **`main`**, mappe: **`/ (root)`**. Klikk **"Save"**.
5. Vent ~1 minutt. Toppen av siden viser:
   `Your site is live at https://DITT-BRUKERNAVN.github.io/poengliste-17mai/`

Det er den lenken du sender til gutta.

---

## Steg 8 — Send til gutta

Send dem lenken. På iPhone:

1. Åpne lenken i **Safari** (må være Safari).
2. Trykk **del-knappen** (firkanten med pil opp).
3. Scroll ned, trykk **"Legg til på Hjem-skjerm"**.
4. Klikk **"Legg til"**. Ferdig — nå er den en app.

Alle ser hverandres poeng i sanntid.

---

## Oppdatere appen senere

```
cd ~/poengliste-17mai
```

```
git add . && git commit -m "oppdatering" && git push
```

GitHub Pages oppdaterer seg selv på ~1 minutt.

## Nullstille alle spillere etter dagen

I Firestore-konsollen: gå til collection `poengliste-2026` og slett dokumentene. Eller la dem ligge til neste år og bytt collection-navn i `index.html`.

---

## Sikkerhet (les hvis du bryr deg)

Reglene er åpne — alle som har URL-en kan lese og skrive. Det går fint for en privat venneklikk, men ikke del lenken offentlig. Hvis du vil ha det strengere, kan du legge på en passordsjekk i app-en eller bruke Firebase Auth — men det er overkill her.
