Implementa **DAS-142 — MOBILE: shell iOS FungoGo aderente al mockup approvato**, partendo rigorosamente dall’ultimo `master`.

Il mockup mobile allegato è il riferimento UX vincolante. Non reinterpretarlo come semplice riduzione responsive della dashboard desktop: FungoGo mobile deve essere progettata per l’utilizzo sul campo con una mano, mantenendo però il frontend React/Vite condiviso e senza introdurre una seconda applicazione.

## 1. Pre-flight obbligatorio

Prima di modificare file:

1. verifica branch, SHA e pulizia del worktree;
2. esegui `git fetch` e `git pull --ff-only` se il remote è disponibile;
3. verifica che la baseline includa semanticamente:

   * DAS-136 foundation Capacitor;
   * DAS-140 handoff mobile;
   * DAS-141 identità FungoGo;
   * DAS-151 export compliance;
   * DAS-152 connettività e geolocalizzazione;
   * DAS-156 purpose string iOS;
   * DAS-157 login iOS, timeout assessment e correzione CTA Monitoraggio;
   * PR #201, che abilita l’handoff nella build TestFlight;
   * PR #202, che instrada le letture di Saved Places e Monitoring attraverso il trasporto API canonico.
4. verifica l’identità iOS:

   * display name `FungoGo`;
   * bundle ID `app.fungogo.ios`;
   * URL scheme `fungogo`;
   * callback `fungogo://auth/callback`;
5. verifica che l’unica Alembic head resti quella prevista;
6. crea il branch:
   `das-142-fungogo-mobile-shell`;
7. se PR #202 non è presente nella baseline, fermati senza implementare e segnala il blocker;
8. non sovrascrivere modifiche estranee del maintainer.

## 2. Audit repository-grounded

Individua e documenta gli owner esistenti di:

* layout e navigazione React;
* header e `AuthControl`;
* mappa e selezione della posizione;
* form assessment;
* stato health/backend;
* sessione e autenticazione mobile;
* `httpTransport` e `CapacitorHttp`;
* geolocalizzazione Capacitor;
* Saved Places;
* Monitoring;
* safe area e configurazione Capacitor;
* design token, colori, typography, spacing e componenti UI;
* workflow iOS Simulator/TestFlight;
* gestione degli stati loading, errore, offline, anonimo e autenticato.

Estendi gli owner esistenti. Non creare:

* un secondo frontend;
* un design system parallelo;
* client API duplicati;
* un secondo owner della sessione;
* una WebView generica per il login;
* route backend alternative;
* componenti scientifici paralleli.

## 3. Obiettivo UX

Realizza una vera shell mobile FungoGo aderente al mockup allegato e ottimizzata per:

* iPhone;
* utilizzo outdoor;
* consultazione con una mano;
* azioni principali raggiungibili con il pollice;
* testi e controlli leggibili anche in movimento;
* safe area, notch e Dynamic Island;
* tastiera virtuale;
* connessione instabile;
* orientamento portrait come esperienza primaria;
* accessibilità e Dynamic Type.

La PWA desktop deve mantenere il layout desktop esistente e non deve essere trasformata nella shell mobile.

## 4. Navigazione mobile

Implementa la navigazione primaria rappresentata nel mockup, adattandola alle funzioni reali presenti nel repository.

La shell deve prevedere accesso chiaro almeno a:

1. **Esplora/Analizza**

   * mappa;
   * posizione corrente;
   * ricerca località;
   * scelta specie;
   * avvio assessment;

2. **Luoghi salvati**

   * contenuti reali esistenti;
   * stato anonimo;
   * loading;
   * empty state;
   * errore;
   * backend non raggiungibile;

3. **Monitoraggio**

   * contenuti reali esistenti;
   * nessun secondo pulsante “Accedi”;
   * autenticazione posseduta unicamente dal controllo globale;
   * stati anonimo, autenticato, loading, errore ed empty;

4. **Attività/Tracce**

   * extension point per la futura registrazione tracce;
   * nessuna simulazione se il runtime non esiste ancora;

5. **Profilo/Altro**

   * account;
   * login/logout;
   * impostazioni;
   * spazio futuro per Wiki e Supporto.

Non aggiungere tab inutili solo per replicare visivamente il mockup. Se il mockup contiene una funzione non ancora implementata, rappresentala come elemento chiaramente “In arrivo”, non interattivo e accessibile, oppure omettila motivando la decisione.

## 5. Home mobile

La home mobile deve privilegiare l’azione principale:

**Analizza un punto**

e rendere immediatamente disponibili:

* uso della posizione corrente;
* ricerca di una località;
* selezione manuale sulla mappa;
* specie target;
* stato del backend;
* messaggi di errore recuperabili;
* accesso ai risultati recenti, solo se realmente disponibile.

Non trasferire nella home mobile tutta la densità informativa della dashboard desktop.

Riduci:

* testi introduttivi troppo lunghi;
* pannelli decorativi;
* duplicazioni;
* scroll prima dell’azione principale;
* informazioni secondarie sopra la piega.

Mantieni però avvertenze scientifiche, affidabilità e provenance dove necessarie: non sacrificare il significato scientifico per comprimere l’interfaccia.

## 6. Mappa e form assessment

Sul mobile:

* la mappa deve essere leggibile senza overflow orizzontale;
* zoom, pan e selezione devono restare utilizzabili;
* i controlli non devono sovrapporsi alla bottom navigation o alle safe area;
* “Usa la mia posizione” deve mostrare chiaramente stato di caricamento e impedire richieste concorrenti;
* permesso negato, timeout e posizione indisponibile devono essere recuperabili;
* ricerca località e coordinate manuali devono restare disponibili;
* il testo digitato non equivale a una località selezionata finché non possiede coordinate;
* il blocker del pulsante assessment deve essere esplicito;
* il pulsante deve essere disabilitato durante l’invio;
* il doppio tap non deve generare richieste duplicate;
* il `request_id` canonico deve restare stabile nei retry espliciti previsti.

Non correggere in DAS-142 il timeout backend ancora aperto in DAS-157 mediante un ulteriore aumento arbitrario del timeout. La shell deve soltanto mostrare correttamente lo stato e consentire il recupero.

## 7. Autenticazione

Preserva integralmente DAS-157 e PR #201:

* `ASWebAuthenticationSession`;
* ZITADEL;
* Authorization Code + PKCE;
* callback `fungogo://auth/callback`;
* handoff monouso backend-owned;
* sessione HttpOnly;
* ritorno automatico nell’app;
* restore;
* logout;
* validazioni fail-closed.

Non incorporare username/password ZITADEL nella UI React e non usare una WebView generica.

Deve esistere un solo controllo globale di autenticazione. Le singole schermate possono mostrare uno stato contestuale anonimo, ma non devono duplicare la CTA “Accedi”.

## 8. Correzioni apprese dalle prove reali

La nuova shell non deve reintrodurre i problemi già osservati:

* ritorno del login nel browser esterno;
* mancato aggiornamento dello stato autenticato;
* pulsante Accedi duplicato;
* freeze durante la geolocalizzazione;
* backend indicato come irraggiungibile per chiamate relative errate;
* richieste a `127.0.0.1:8000`;
* utilizzo diretto di `fetch("/api/...")` nei client privati;
* CTA assessment priva di spiegazione;
* perdita dello stato dopo errore;
* conflitti tra safe area e navigazione;
* controlli coperti dalla tastiera;
* funzioni inesistenti presentate come operative.

Ogni nuova lettura API deve usare il trasporto canonico e rispettare `VITE_API_BASE_URL`.

## 9. Funzioni future ed extension point

Predisponi nel layout, senza implementarne falsamente il runtime:

* registrazione traccia GPS;
* pausa/ripresa/termine traccia;
* waypoint;
* scatto e associazione fotografie;
* salvataggio del luogo;
* import/export GPX;
* griglia predittiva;
* companion Apple Watch DAS-158;
* Wiki FungoGo DAS-153;
* Supporto utenti DAS-154.

Gli extension point devono essere documentati e collegati agli owner esistenti. Evita pulsanti attivi che producano toast di successo senza effettuare realmente l’operazione.

## 10. Migliorie consentite rispetto al mockup

Puoi introdurre aggiustamenti motivati quando migliorano concretamente:

* safe area;
* accessibilità;
* leggibilità outdoor;
* dimensione minima dei touch target;
* contrasto;
* tastiera;
* gestione degli errori;
* stato offline;
* gerarchia delle CTA;
* riduzione di duplicazioni;
* coerenza con funzionalità realmente disponibili.

Ogni divergenza rilevante dal mockup deve essere elencata nel documento DAS-142 con:

* elemento del mockup;
* implementazione adottata;
* motivazione;
* impatto desktop/mobile;
* eventuale DAS futura.

Non modificare liberamente l’identità visiva approvata. Usa FungoGo come nome pubblico. Se gli asset definitivi DAS-155 non sono ancora disponibili nel repository, riusa temporaneamente gli asset esistenti senza inventare un nuovo logo.

## 11. Responsive e desktop

Definisci breakpoint motivati e verifica almeno:

* iPhone compatto;
* iPhone standard;
* iPhone grande;
* mobile browser;
* tablet;
* desktop.

Desktop e mobile devono condividere logica, API e componenti di dominio, ma possono avere shell e composizione visiva differenti.

Non degradare:

* dashboard desktop;
* mappa desktop;
* account;
* Saved Places;
* Monitoring;
* assessment;
* login web;
* navigazione da tastiera.

## 12. Accessibilità

Verifica almeno:

* touch target adeguati;
* ordine del focus;
* focus visibile;
* label accessibili;
* VoiceOver;
* Dynamic Type;
* contrasto;
* reduced motion;
* messaggi di errore annunciabili;
* bottom navigation semanticamente corretta;
* nessuna informazione affidata esclusivamente al colore;
* tastiera e controlli non sovrapposti.

Il semaforo backend deve avere anche testo/stato accessibile, non soltanto il colore.

## 13. Test richiesti

Aggiungi o aggiorna test per:

* shell mobile e desktop;
* navigazione primaria;
* tab attivo;
* safe area;
* stati backend online/offline;
* autenticazione anonima/autenticata;
* assenza di CTA Accedi duplicate;
* Saved Places;
* Monitoring;
* geolocalizzazione loading/success/denied/timeout;
* località digitata ma non selezionata;
* blocker assessment;
* submit e doppio tap;
* errori assessment;
* placeholder futuri non interattivi;
* accessibilità minima;
* nessuna regressione desktop;
* contratto iOS dopo `cap sync`.

Esegui almeno:

```bash
uv sync --all-extras --locked
uv run python -m pytest
uv run python -m src.quality --quick

cd frontend
npm ci
npm test
npm run lint
npm run build
npm run build:mobile

VITE_API_BASE_URL=https://api-staging.fungogo.app \
VITE_MOBILE_AUTH_HANDOFF_ENABLED=true \
npm run build:mobile:staging

npx cap sync ios
```

Esegui lint focalizzato su tutti i file modificati. Se il lint completo conserva esclusivamente la baseline nota dei quattro errori `no-explicit-any` nei file E2E non modificati, documentala senza presentarla come successo completo. Ogni nuovo errore è bloccante.

Verifica inoltre:

* assenza di URL `127.0.0.1:8000` negli asset mobile/staging;
* presenza di `https://api-staging.fungogo.app` negli asset mobile;
* assenza di secret, token, cookie, handoff e materiale Apple;
* plist valida;
* singola Alembic head;
* `git diff --check`.

Se possibile, acquisisci screenshot comparativi del mockup e dell’implementazione ai principali breakpoint. Non dichiarare confronto visivo completato se l’ambiente non consente il rendering.

## 14. Workflow e TestFlight

Estendi il workflow iOS canonico soltanto se necessario per verificare la shell. Non creare workflow paralleli.

Mantieni:

* Xcode/iPhoneOS SDK 26+ nel percorso TestFlight;
* bundle ID `app.fungogo.ios`;
* display name `FungoGo`;
* scheme Xcode canonico `App`;
* URL scheme `fungogo`;
* export compliance;
* purpose string location;
* cleanup del materiale di firma;
* Environment protetto `testflight`;
* `VITE_MOBILE_AUTH_HANDOFF_ENABLED=true` esclusivamente nel job TestFlight controllato.

Non eseguire upload TestFlight, deploy Railway, modifiche ZITADEL, modifica DNS o merge senza autorizzazione.

## 15. Documentazione

Crea o aggiorna:

* `docs/DAS_142_FUNGOGO_MOBILE_SHELL.md`;
* `ROADMAP.md`;
* `architecture_audit.md`;
* eventuale checklist mobile/TestFlight canonica già esistente.

Documenta:

* confronto con il mockup;
* componenti riusati;
* componenti introdotti;
* differenze mobile/desktop;
* extension point futuri;
* test;
* limitazioni;
* rischi;
* rollback;
* gate Simulator;
* gate TestFlight/iPhone;
* assenza di modifiche scientifiche.

Non modificare retroattivamente le evidenze storiche delle DAS precedenti.

## 16. Criteri di accettazione

DAS-142 può essere proposta per il merge quando:

* la shell mobile rispecchia il mockup approvato;
* le divergenze sono motivate;
* il desktop non regredisce;
* funzioni reali e future sono chiaramente distinguibili;
* login DAS-157 rimane funzionante;
* le letture API non usano proxy localhost;
* geolocalizzazione non blocca la UI;
* Monitoraggio non duplica Accedi;
* safe area, tastiera e scroll sono verificati;
* test e build richiesti sono superati;
* non sono presenti secret o artifact Apple.

DAS-142 non è **Released** fino a quando:

* il branch è stato mergiato;
* lo smoke Simulator su `master` è verde;
* una nuova build TestFlight è stata processata;
* il maintainer ha installato la build;
* la shell è stata verificata su iPhone reale.

## 17. Git e consegna

1. mantieni il lavoro in un branch dedicato;
2. produci commit coerenti e non usare amend su commit già pubblicati;
3. non eseguire merge;
4. crea una PR verso `master`;
5. restituisci:

   * baseline;
   * branch;
   * commit e SHA;
   * URL reale della PR, se creato;
   * file modificati;
   * componenti riusati/introdotti;
   * test con esito esatto;
   * limitazioni ambientali;
   * confronto mockup/implementazione;
   * rischi e rollback;
   * checklist manuale per Simulator e iPhone.

Se remote, credenziali, macOS, Xcode, Simulator, TestFlight, Railway, ZITADEL o Jira non sono disponibili, indica esattamente:

`NOT RUN — ENVIRONMENT CAPABILITY MISSING`

Non simulare risultati, non inventare URL di PR e non dichiarare Released una funzionalità non verificata su iPhone.
