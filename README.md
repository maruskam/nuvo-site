# nuvo-site

Il sito di [Nuvo](https://github.com/maruskam) — app per annotare le spese di
casa a voce o dalla foto di uno scontrino. Due pagine statiche, senza build e
senza dipendenze: il repo è già la radice del sito da pubblicare.

```
index.html          il sito
privacy.html        l'informativa privacy (campo «Privacy Policy URL»)
supporto.html       la pagina di assistenza (campo «Support URL», obbligatorio)
favicon.svg         icona della scheda
robots.txt          indicizzazione libera
_headers            cache dei font (usato da Cloudflare Pages, ignorato da GitHub Pages)
.nojekyll           evita che GitHub Pages processi i file
fonts/              Baloo 2 e Nunito (woff2 variabili) + licenze OFL
img/                ritagli dell'app usati nella pagina di assistenza
```

**Nessuna richiesta a terzi.** I font sono serviti dal sito, non da Google: le
pagine non contattano nessun host esterno, quindi non trasferiscono l'IP dei
visitatori a nessuno e non serve un banner cookie. Sono gli stessi file
variabili impacchettati nell'app, sottoinsieme latino + latino esteso, licenza
SIL OFL 1.1 (`fonts/OFL-*.txt`).

I riferimenti interni sono tutti relativi, quindi il sito funziona sia sulla
radice di un dominio sia in una sottocartella tipo `/nuvo-site/`.

## Prova in locale

```bash
python3 -m http.server 4173
```

Poi apri <http://localhost:4173>. Apri via server e non con `file://`, altrimenti
i font non caricano.

## Pubblicazione — GitHub Pages

```bash
gh repo create nuvo-site --public --source=. --remote=origin --push
```

Poi: Settings → Pages → Source `Deploy from a branch`, branch `main`, cartella
`/ (root)`. In un paio di minuti il sito è su:

```
https://maruskam.github.io/nuvo-site/
https://maruskam.github.io/nuvo-site/privacy.html    ← Privacy Policy URL
https://maruskam.github.io/nuvo-site/supporto.html   ← Support URL
```

Gratis, HTTPS incluso, nessun dominio da comprare. Da qui in avanti ogni
`git push` pubblica.

### Con un dominio tuo (opzionale)

1. Crea un file `CNAME` con dentro solo il dominio, per esempio `nuvo-app.it`.
2. Settings → Pages → Custom domain, e spunta *Enforce HTTPS* quando il
   certificato è pronto.
3. Record DNS presso il registrar:

| Tipo | Nome | Valore |
|---|---|---|
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| CNAME | `www` | `maruskam.github.io` |

Se il DNS è su Cloudflare, tieni i record su **DNS only** (nuvola grigia)
finché GitHub non ha emesso il certificato.

## Pubblicazione — Cloudflare Pages (alternativa)

Workers & Pages → Create → Pages → collega il repo. Framework preset `None`,
build command **vuoto**, output directory `/`. Ti dà un sottodominio con il
nome che scegli tu (`nuvo-app.pages.dev`), pubblica anche da repo privati e
usa il file `_headers`.

## Quando cambi indirizzo

Se passi dal sottodominio a un dominio tuo, aggiorna l'URL in App Store
Connect ma **lascia in piedi la vecchia pagina** con un redirect, così i link
già in circolazione non muoiono. Su GitHub Pages non ci sono redirect lato
server: basta sostituire il contenuto di `privacy.html` con

```html
<meta http-equiv="refresh" content="0; url=https://nuvo-app.it/privacy.html">
```

## Da completare prima di pubblicare

- `index.html` e `supporto.html`: quando esce la versione Android, aggiungere
  il link a Google Play accanto a quello dell'App Store e aggiornare la
  risposta «Su quali telefoni funziona?».
- Testi: mai il trattino lungo «—»; al suo posto virgole, due punti o parentesi.
- `privacy.html`: aggiornare la data in alto a ogni modifica sostanziale.
- `supporto.html`: se cambi l'indirizzo email, va aggiornato anche qui (tre punti)
  oltre che nell'informativa.
