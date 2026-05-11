# Script utili

Esempio di citazione in linea \cite{site:agile-manifesto}. \\

Esempio di citazione nel pie' di pagina citazione\footcite{womak:lean-thinking}
\\

# Capitoli da sistemare

- Implementazione test
- Aggiornare tecnologie adottate
- Aggiornare diagramma delle classi
- Rileggere tutto per vedere se si parla anche di test

# To do

- Integrare parte di scelta e studio delle tecnologie con i criteri di scelta e
  i pro e contro di ogni tecnologia
- Togliere RFID dal glossario e metterlo a piè di pagina con una breve
  spiegazione
- Introdurre skill claude code (e spiegarle una a una)
- Sostituire immagine architettura otel collector con una generale con loghi
  delle tecnologie
- Introdurre immagini delle dashboard per far capire

- Sistemare descrizione capitoli (sia intro di ciascuno che introduzione a
  inizio tesi)
- Controllare che i riferimenti ai requisiti siano giusti (sezione scelte
  architetturali)
- Evidenziare i termini in linguaggio tecnico con il corsivo
- Finito il glossario, trovare tutti i termini nel documento con Ctrl+F e
  sostituirli con \gls{termine}

# Modifiche apportate (revisione del 2026-05-05)

Revisione complessiva della tesi guidata da quattro direttive: (1) valorizzare i
contributi originali del lavoro di stage, (2) migliorare prosa, lessico e
sintassi, (3) spostare in appendice i contenuti tecnici corposi mantenendo nel
corpo principale descrizioni di alto livello con rimando, (4) rendere
l'introduzione meno tecnica.

## 1. Introduzione meno tecnica + valorizzazione dei contributi

- `chapters/introduzione.tex`: riscritta interamente.
  - Sezione "L'idea" rifatta in chiave narrativa: parte dal problema reale di
    Sia.Core (compromesso stabilità/evoluzione, difficoltà di diagnosi nei
    sistemi distribuiti) invece che dai dettagli tecnici di OpenTelemetry.
  - Nuova sezione "Una metodologia di lavoro centrata sull'AI" che introduce
    workflow Plan->Confirm->Implement, skill procedurali e MCP come elementi
    originali.
  - Nuova sezione "Contributi originali" (`\label{sec:contributi}`) che enuncia
    esplicitamente i cinque contributi del lavoro, da richiamare nelle
    conclusioni.
  - Sezione "Organizzazione del testo" aggiornata per riflettere il rinvio in
    appendice dei contenuti pesanti.

- `preface/summary.tex`: sommario riscritto.
  - Migliorata la prosa, eliminato il refuso "obbiettivi".
  - Esplicitati il contesto aziendale, il doppio filone (telemetria + testing),
    l'originalità della metodologia AI-assistita.

## 2. Spostamento contenuti tecnici corposi in appendice

- `appendix/appendice-a.tex`: riscritto da zero, ora articolato in tre capitoli
  di appendice.
  - **Appendice A** (`\label{app:requisiti}`): tabelle complete dei requisiti
    funzionali, qualitativi e di vincolo (`tab:requisiti-funzionali`,
    `tab:requisiti-qualitativi`, `tab:requisiti-vincolo`).
  - **Appendice B** (`\label{app:diagrammi-classi}`): i tre diagrammi UML in
    TikZ (`fig:diag-pipeline`, `fig:diag-diagnostics-pattern`,
    `fig:diag-enduser`) con descrizione di dettaglio.
  - **Appendice C** (`\label{app:listati}`): listati di codice del pattern
    Adapter (`lst:adapter-diagnostics-app`, `lst:recordmetricerror-call-app`).

- `chapters/concept.tex`: rimosse le tre tabelle di tracciamento dei requisiti
  (RFN/RQN/RVN). Sostituite da una descrizione di alto livello della struttura
  del codice identificativo, con rinvio all'appendice~\ref{app:requisiti}.
  Aggiunta `\label{sec:codice-requisiti}` per il rimando dall'appendice.

- `chapters/progettazione.tex`:
  - Rimossa la sezione "Diagrammi delle classi del sistema di telemetria" con le
    tre figure TikZ estese (~250 righe). Sostituita da un elenco puntato che
    descrive cosa rappresenta ciascun diagramma, con rinvio
    all'appendice~\ref{app:diagrammi-classi}.
  - Rimossa la duplicazione della sottosezione "Pattern Adapter": prima presente
    due volte con contenuti analoghi (la prima con listati inline, la seconda
    solo testuale). Consolidata in un'unica sottosezione "Pattern Adapter per il
    counter di errore dei controller" con prosa migliorata
    (`\paragraph{Contesto/Problema/Soluzione/Vantaggi}` al posto di
    `\textbf{...}\\`) e rinvio ai listati in appendice.

- `chapters/codifica.tex`: rimossi i due blocchi `verbatim` (definizione della
  classe `SiaDevextremeCrudDiagnostics` e blocco `catch` del controller).
  Sostituiti da descrizione testuale di alto livello con rinvio ai listati in
  appendice~\ref{app:listing-adapter}.

## 3. Valorizzazione dei contributi nelle conclusioni

- `chapters/conclusioni.tex`: aggiunta nuova sezione "Sintesi dei contributi
  originali" prima del raggiungimento degli obiettivi. La sezione articola in
  quattro paragrafi:
  - progettazione e integrazione del sistema di osservabilità centralizzato;
  - catena diagnostica `metrica -> trace -> utente -> log` (enduser + exemplar);
  - nascita dell'infrastruttura di testing, con menzione esplicita dei due bug
    applicativi latenti scoperti dalla prima esecuzione della suite;
  - metodologia AI-assistita disciplinata e replicabile.

## 4. Correzioni minori di consistenza

- `chapters/progettazione.tex`: aggiunte le label `\label{sec:modello-enduser}`
  e `\label{sec:modello-exemplars}` alle rispettive sottosezioni. Erano
  referenziate da `progettazione.tex` e `conclusioni.tex` ma non definite
  (riferimenti orfani nell'originale).

## File NON modificati

I seguenti file sono stati lasciati invariati perché già coerenti con le
direttive o di taglio descrittivo non spostabile in appendice: `structure.tex`,
`printable-thesis.tex`, `chapters/kick-off.tex`, `chapters/tecnologie.tex`,
`chapters/metodologia-ai.tex`, `chapters/test-implementazione.tex` (peraltro non
incluso in `structure.tex`), tabelle compatte di tag/attributi nel corpo di
`progettazione.tex`, file di `config/`, `preface/` (eccetto `summary.tex`),
glossary.
