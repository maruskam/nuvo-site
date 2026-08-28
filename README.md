# Sito di Nuvo

Due pagine statiche, senza build e senza dipendenze: si pubblicano copiando
la cartella così com'è.

```
index.html          il sito
privacy.html        l'informativa privacy (URL da dare ad App Store Connect)
favicon.svg         icona della scheda
robots.txt          indicizzazione libera
.nojekyll           evita che GitHub Pages processi i file
fonts/              Baloo 2 e Nunito (woff2 variabili) + licenze OFL
```

**Nessuna richiesta a terzi.** I font sono serviti dal dominio, non da Google:
la pagina non contatta nessun host esterno, quindi non trasferisce l'IP dei
visitatori a nessuno e non ha bisogno di banner cookie. Sono gli stessi file
variabili impacchettati nell'app (`assets/fonts/`), sottoinsieme latino +
latino esteso, licenza SIL OFL 1.1.

Verificato in locale: `document.fonts` entrambi `loaded`, zero risorse esterne
in `performance.getEntriesByType('resource')`.

## Prova in locale

```bash
python3 -m http.server 4173 --directory site
```

Poi apri <http://localhost:4173>. Servono i percorsi relativi, quindi apri via
server: con `file://` i font non caricano.

## Pubblicazione — GitHub Pages

1. Crea un repo (per esempio `nuvo-site`) e mettici **il contenuto** di questa
   cartella alla radice, non la cartella stessa.
2. Aggiungi un file `CNAME` con dentro solo il dominio, per esempio
   `nuvo-app.it`.
3. Settings → Pages → Source: `Deploy from a branch`, branch `main`, cartella
   `/ (root)`.
4. Spunta *Enforce HTTPS* appena il certificato è pronto (pochi minuti).

DNS da impostare presso il registrar (dominio senza `www`, cioè apex):

| Tipo | Nome | Valore |
|---|---|---|
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| CNAME | `www` | `<utente>.github.io` |

Se il DNS è su Cloudflare, tieni i record su **DNS only** (nuvola grigia)
almeno fino a quando GitHub ha emesso il certificato.

## Pubblicazione — Cloudflare Pages

1. Dashboard Cloudflare → Workers & Pages → Create → Pages → collega il repo.
2. Framework preset: `None`. Build command: **vuoto**. Output directory: `/`.
3. Custom domains → aggiungi il dominio: se è già su Cloudflare, i record li
   crea da sé.

Non serve il file `CNAME` (è una cosa di GitHub Pages) e puoi cancellarlo.

## Email sul dominio

Cloudflare → Email → Email Routing: crea `ciao@tuodominio` inoltrata alla
Gmail attuale, gratis. Quando è attiva ricordati di aggiornare l'indirizzo in
`privacy.html` (punti 1, 9 e 13), nei link `mailto:` di `index.html` e nel
campo *Email di feedback* di App Store Connect.

## Da completare prima di pubblicare

- `privacy.html`, punto 1: sostituire i segnaposto evidenziati con nome (o
  ragione sociale) e indirizzo del titolare del trattamento.
- `index.html`: quando l'app esce dalla beta, sostituire i pulsanti «Prova la
  beta» / «Chiedi l'invito» con i link ad App Store e Google Play, e
  aggiornare la risposta «Su quali telefoni funziona?».
- `privacy.html`: aggiornare la data in alto a ogni modifica sostanziale.
