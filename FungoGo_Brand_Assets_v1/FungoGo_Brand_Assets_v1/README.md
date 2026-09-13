# FungoGo — Brand Assets v1
Master visual approvata: 11 settembre 2026.

## Regola fondamentale
La master approvata NON va reinterpretata. La parte “Fun” deve restare invariata:
- lettering avorio/beige;
- porcino integrato nella U;
- gambo come prosecuzione naturale della U.
La parte “GO” resta verde lime con freccia di navigazione nella G e tartufo luminoso dentro la O.
Sfondo verde bosco con curve topografiche, percorso tratteggiato e pin.

## Contenuto pacchetto
00_MASTER
- Master approvata, sorgente canonica raster.

01_iOS
- 1024x1024: sorgente/marketing icon.
- Export legacy/QA: 20, 29, 40, 58, 60, 76, 80, 87, 120, 152, 167, 180 px.
Nota: per la pipeline Capacitor/Xcode moderna usare preferibilmente la master 1024 e lasciare la generazione all'asset pipeline.

02_Android
- Launcher legacy: mdpi 48, hdpi 72, xhdpi 96, xxhdpi 144, xxxhdpi 192.
- Google Play listing: 512x512.
Nota: per Adaptive Icons Android conviene generare in implementazione foreground/background dedicati e testare safe-zone/mask; non è corretto fingere che una semplice resize sia un adaptive icon.

03_PWA_Web
- 64, 128, 144, 180, 192, 256, 384, 512 px.
- 192 e 512 sono i principali manifest icons.
- 180 è utile come Apple touch icon.

04_Desktop_Favicon
- PNG 16, 32, 48, 64, 128, 256.
- favicon.ico multi-size.

05_Social_Preview
- Open Graph 1200x630.
- Social 1200x675.
L'icona approvata è solo posizionata su fondo neutro; non viene reinterpretata.

06_Splash
- Source quadrata 2732x2732.
- iPhone portrait 1290x2796.
- iPad portrait 2048x2732.
Sono basi di branding; la pipeline nativa deve generare/verificare le varianti richieste dai device target.

## Verifiche prima della release
1. Test icona a 16/32/60/120 px: tartufo e porcino devono restare riconoscibili.
2. Verifica iOS masks / App Store asset validation.
3. Verifica Android circle/squircle/rounded-square masks.
4. Verifica PWA manifest e installazione desktop/mobile.
5. Nessun testo aggiuntivo dentro l'icona.
6. Conservare la master approvata come unica source of truth.
