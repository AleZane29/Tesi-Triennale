# Script utili

Esempio di citazione in linea \cite{site:agile-manifesto}. \\

Esempio di citazione nel pie' di pagina citazione\footcite{womak:lean-thinking}
\\

# To do

- Integrare parte di scelta e studio delle tecnologie con i criteri di scelta e
  i pro e contro di ogni tecnologia
- Togliere RFID dal glossario e metterlo a piè di pagina con una breve
  spiegazione
- Introdurre skill claude code (e spiegarle una a una)
- Sostituire immagine architettura otel collector con una generale con loghi
  delle tecnologie
- Introdurre immagini delle dashboard per far capire

- Capire se reintrodurre sezione verifica e validazione (\intro{Il presente
  capitolo illustra le attività di verifica e validazione condotte durante lo
  sviluppo del progetto, analizzando i test eseguiti ai vari livelli, dai test
  di unità fino alle fasi finali di collaudo e accettazione del prodotto.})
- Aggiornare in modello delle metriche il tipo di metriche usate (oltre a
  counter e histogram)

- Sistemare descrizione capitoli (sia intro di ciascuno che introduzione a
  inizio tesi)
- Controllare che i riferimenti ai requisiti siano giusti (sezione scelte
  architetturali)
- Evidenziare i termini in linguaggio tecnico con il corsivo
- Finito il glossario, trovare tutti i termini nel documento con Ctrl+F e
  sostituirli con \gls{termine}

# Aggiunte

---

# Revisione della tesi — correzioni e aggiunte suggerite

> Note generate dopo lettura dei sorgenti LaTeX (`chapters/*.tex`), dell'agente
> `sia-telemetry-developer` e delle skill `sia-telemetry-*` del progetto
> Sia.Web. Di seguito vengono segnalati errori, incongruenze e proposte di
> integrazione, capitolo per capitolo, più i nuovi capitoli richiesti.

## Correzioni trasversali (tutto il documento)

- **Uniformare apostrofi e accenti**: nel sorgente compaiono forme miste
  `\`e`/`è`, `\'u`/`ù`, `perch\'e`/`perché`, `pi\`u`/`più`. Scegli una forma
  (preferisci gli accenti Unicode diretti `è à ù ò ì é`) e rendila coerente.
- **Virgolette**: alterni `"..."` dritte e `“...”` curve. In italiano scientifico
  usa `«...»` oppure uniforma a `“...”` tramite pacchetto `csquotes`.
- **Termini tecnici**: la convenzione dichiarata nell'intro è "termini in lingua
  straniera in corsivo + prima occorrenza come `\gls{}`". Molti termini però
  non sono in corsivo (es. *backend*, *frontend*, *trace*, *span*, *counter*,
  *histogram*, *bucket*, *opt-in*, *staging*, *rate*, *throughput*, *LGTM*).
  Passata finale con Ctrl+F obbligatoria.
- **Acronimi da aggiungere al glossario**: MELT, CNCF, OTLP, LGTM, SDK (già?),
  CRUD (già?), 2FA/TOTP, SPA (già?), API (già?), CQRS, SIEM (se citato),
  \textit{p50/p95/p99} (spiegare che sono percentili), \textit{throughput},
  \textit{sampler}, \textit{processor}, \textit{exemplar}, \textit{baggage}.
- **Didascalie figure duplicate**: tutte le figure hanno
  `\label{fig:placeholder}`. Vanno rinominate univocamente (es.
  `fig:logo-sia`, `fig:logo-angular`, `fig:arch-otel-collector`, ecc.),
  altrimenti i riferimenti falliscono.
- **Riferimenti ai requisiti**: nella sezione "Scelte architetturali" si cita
  `RNF5` e `RNF6` ma nel capitolo requisiti i requisiti sono numerati come
  `RNF1..RNF6` senza un mapping chiaro → controlla che `RNF5` sia davvero
  "compatibilità OTLP/OTel backend" e `RNF6` sia "attivabile via appsettings".
  Coerentemente con quanto scritto ora: `RNF5` sembra essere "segnali
  compatibili con backend OTel" (corretto) e `RNF6` "appsettings per
  ambiente" (corretto), ma allora l'attivazione opt-in riferita in
  progettazione punta al requisito sbagliato: è `RF5` (attivabile via
  configurazione) non `RNF6`.

## Capitolo 1 — Introduzione

- `\label{fig:placeholder}` duplicato (vedi sopra).
- Frase "*Continua con descrizione tecnica...*" residua nel cap. 2 → da
  rimuovere/completare.
- Elenco "Organizzazione del testo": il quinto e sesto capitolo hanno il testo
  troncato (`espone ...`, `descrive ...`). Completali con una frase ciascuno.
- La descrizione dell'idea è incentrata su *Spec-Kit + testing*, ma il corpo
  della tesi tratta principalmente la **telemetria**. Ribilanciare l'introduzione:
  dire esplicitamente che le due linee (testing AI-driven e osservabilità)
  sono state condotte in parallelo, e che il focus del documento è soprattutto
  la seconda.
- Aggiungere una frase sulla **collaborazione uomo-agente** (Claude Code) come
  metodologia abilitante, da riprendere poi nel capitolo dedicato.

## Capitolo 2 — Descrizione dello stage

- `\intro{...}` iniziale elenca "obiettivi, pianificazione, rischi"; nel testo
  però compare anche la sezione "Introduzione al progetto" con teoria
  dell'osservabilità → spostare quella teoria nel capitolo Progettazione
  (dove è già ripresa) per evitare duplicazione.
- Requisiti funzionali:
  - `RF2` ("attributi che espongono metodo e repository"): descrivere meglio
    — si tratta dei tag `sia.method`, `sia.caller_method`, `sia.repository`.
  - `RF8` ("Sviluppo di Claude skill"): chiarire che si riferisce alle skill
    `sia-telemetry-setup/traces/metrics/logs/grafana` usate per propagare la
    telemetria negli altri moduli. Manca nel glossario la voce "Claude skill".
  - Aggiungere un requisito funzionale esplicito:
    - `RF9` — *Il sistema deve arricchire automaticamente ogni span con
      l'identità dell'utente autenticato (`enduser.id`, `enduser.name`) senza
      richiedere modifiche ai singoli moduli.*
    - `RF10` — *Le metriche di tipo Histogram devono esporre exemplars
      (puntatori al `TraceId` del campione) per abilitare la navigazione
      metrica → trace.*
- Requisiti non funzionali: `RNF1` "Identificazione delle tecnologie" è più un
  obiettivo che un RNF; sposta in "Obiettivi".
- Pianificazione: le settimane 6, 7 e 8 hanno lo **stesso titolo**
  ("Iterazione e integrazione della telemetria"). Rinomina:
  - Settimana 6 → "Iterazione sui test e prima integrazione della telemetria"
  - Settimana 7 → "Validazione del sistema integrato e tuning dashboard"
  - Settimana 8 → "Messa in produzione e documentazione finale"
- La pianificazione cita ripetutamente *OpenTelemetry* senza menzionare le
  attività con **agenti AI** (studio accelerato, PoC, espansione telemetria).
  Vedi nuovo capitolo proposto più sotto.
- Refuso nel box `risk:test-quality`: "*test generati tramite modelli di
  intelligenza artifutilizzo di un approccio incrementale all'integrazione,
  partendo da coiciale*" → frase corrotta da fondere/ripulire.

## Capitolo 3 — Analisi dei requisiti

- "Continua con descrizione tecnica..." è un TODO lasciato nel sorgente.
- La sezione "Tracciamento dei requisiti" dichiara codice `R(F/Q/V)(N/D/O)` ma
  poi nell'elenco il terzo carattere è `N/D/Z` (Z al posto di O). Uniformare.
- Tabelle del tracciamento: contengono una sola riga placeholder (`RFN-1`,
  `RQD-1`, `RVO-1`). Compilare con i requisiti realmente analizzati e mappare
  agli use case. In particolare mancano i requisiti relativi a telemetria
  (RF1..RF10 della sezione 2) → attualmente non sono tracciati.
- Attori: le descrizioni `LLM` e `Sia.Core` sono "descrizione" (TODO).
  Completare.
- Use case UC0 è l'unico definito; l'elenco casi d'uso però ne cita due
  ("generazione test" e "visualizzazioni dashboard Grafana"). Aggiungere
  almeno UC1 (generazione test) e UC2 (visualizzazione dashboard).
- Aggiungere use case:
  - UC3 *Propagazione della telemetria a un nuovo modulo* (attore:
    sviluppatore; pre: esiste un modulo senza Diagnostics; post: il modulo
    espone span, metriche, dashboard).
  - UC4 *Diagnosi di una richiesta lenta* (attore: sviluppatore/operations;
    flusso: da metrica percentile → exemplar → trace → log correlato).
- Note sulla **cardinalità**: inserire un requisito non funzionale esplicito
  (oggi implicito) che imponga di non usare tag ad alta variabilità come
  `userId`, `orderId` sulle metriche.

## Capitolo 4 — Tecnologie adottate

- Sezione `\gls{poc}` citata ma non c'è struttura: integrare un sottoparagrafo
  "Criteri di selezione" con i criteri concreti usati (maturità, licenza,
  integrazione .NET, vendor-lock-in, community, costo totale di esercizio).
- Per ogni tecnologia aggiungere **pro/contro** e **alternative scartate**:
  - *OpenTelemetry* vs Application Insights / Datadog APM (lock-in, costi,
    completezza delle istrumentazioni .NET).
  - *Grafana LGTM* vs ELK stack, Jaeger standalone (correlazione nativa MELT
    integrata vs assemblaggio manuale).
  - *Serilog* vs Microsoft.Extensions.Logging puro (enrichment, sink OTLP,
    strutturazione).
  - *Spec-Kit* vs prompting manuale / librerie di generazione test
    tradizionali.
- Manca la descrizione di **componenti infrastrutturali** effettivamente
  usati nella tesi: OTel Collector, Tempo, Loki, Prometheus, Mimir (se
  presente), Docker/compose, MCP (già abbozzato in fondo).
- Aggiungere **Claude Code** e **MCP** tra le tecnologie (non è solo un
  protocollo, è lo strumento che abilita il capitolo "tempo ottimizzato").
- La sezione "Studio e scelta delle tecnologie" è molto generica: arricchiscila
  con un paragrafo concreto su come l'uso di un agente AI ha accorciato la
  curva di apprendimento (rimanda al nuovo capitolo).

## Capitolo 5 — Progettazione

Il capitolo è il più solido, ma ha alcuni punti da correggere/espandere.

- Ripetizione della definizione di osservabilità (vedi cap. 2). Tienila qui.
- Tassonomia delle metriche: attualmente solo *Counter* e *Histogram*. Nel
  codice reale sono usati anche:
  - `UpDownCounter` (es. contatori di operazioni in corso / gauge di
    occupazione);
  - `ObservableGauge` / `ObservableCounter` per valori campionati via callback
    (es. numero di connessioni attive, dimensione pool).
  Aggiungere una voce per ciascuno nel paragrafo "Modello delle metriche".
- **Sampler a due livelli**: la descrizione parla di `GlobalRatioParentBasedSampler`
  + `PerSourceFilterProcessor`, ma l'agente telemetry fa riferimento a
  `SiaParentBasedSampler` con ratio per-source. Allinea il nome effettivo delle
  classi rispetto al codice in `Application.Shared.Telemetry`.
- **Campionamento child-based**: aggiungere la nota che le child span ereditano
  sempre dalla decisione del parent, così se `SamplingRatio` globale è 0 ma
  `Sia.Grid` è 1.0 le trace Grid vengono comunque tracciate integralmente.
- Modello dei log: citare il **sink OTLP di Serilog** e il fatto che TraceId e
  SpanId sono propagati tramite `Activity.Current` → vanno automaticamente
  sui log.
- Aggiungere sottosezione **"Arricchimento del contesto utente"** (vedi nuovo
  capitolo più sotto) e sottosezione **"Exemplars per la correlazione
  metrica→trace"** (vedi nuovo capitolo).
- "Scelte architetturali" → rinumera i riferimenti ai requisiti come detto
  sopra (`RF5` invece di `RNF6` per l'opt-in).
- Aggiungere un sottoparagrafo **"Pattern architetturali applicati"** che
  introduca Adapter e Factory prima di spiegarli in dettaglio nel nuovo
  capitolo (vedi sotto).
- Sezioni commentate in coda (`Sviluppo telemetria`, `Sviluppo test`,
  `Design Pattern utilizzati`) vanno trasformate in contenuto vero o tolte.

## Capitolo 6 — Codifica

Attualmente è praticamente vuoto (`\intro{}` e una riga "Attributi di metriche e
trace"). Proposta di struttura concreta:

1. **Organizzazione del codice** (`BaseApplicationBuilder`, `ContainerExtensions`
   per modulo, classi `Sia{Modulo}Diagnostics`).
2. **Pipeline OpenTelemetry** (pacchetti NuGet, `AddOpenTelemetry()`,
   `WithTracing/WithMetrics/WithLogging`, sampler, exporter OTLP).
3. **Pattern `Sia{Modulo}Diagnostics`** (ActivitySource, Meter, counter
   `ControllerErrors`, helper `TrackQuery`).
4. **Pattern standard `RecordMetricError` + `IControllerDiagnostics` con
   Adapter** (contratto dell'interfaccia, adapter annidato `private sealed`,
   invocazione dal controller — esempio di codice su `Sia.Devextreme.Crud`).
5. **Middleware `EnrichActivityWithUserMiddleware` + `BaggageActivityProcessor`**
   (arricchimento automatico di ogni span con `enduser.id` / `enduser.name`).
6. **Configurazione exemplars** (`SetExemplarFilter(ExemplarFilterType.TraceBased)`
   nel pipeline Meter).
7. **Log Serilog OTLP** (configurazione sink, correlazione TraceId/SpanId).
8. **Configurazione dashboard Grafana** (struttura JSON versionata in
   `Diagnostics/{nome}.dashboard.json`, variabili template obbligatorie
   `service_name`, `enduser_id`, `enduser_name`, `bucket`).
9. **Propagazione della telemetria ad altri moduli via skill Claude Code**.

## Capitolo 7 — Conclusioni

Attualmente vuoto (solo i titoli delle sezioni). Spunti:

- **Consuntivo finale**: confronto ore pianificate vs ore effettive, con
  menzione del risparmio ottenuto grazie all'uso di agenti AI nelle fasi di
  studio, PoC e propagazione (→ rimanda al nuovo capitolo).
- **Raggiungimento degli obiettivi**: checkbox su RF1..RF10 e RNF1..RNF*.
- **Conoscenze acquisite**: OpenTelemetry, Grafana, PromQL/TraceQL/LogQL,
  Serilog, design pattern applicati (Adapter, Factory), prompt/skill
  engineering, MCP.
- **Valutazione personale** e **Sviluppi futuri** (es. tail-based sampling,
  RED/USE automatiche, SLO e alerting, profili continui Pyroscope).

## Capitolo "MCP" in coda (oggi "Roba da organizzare")

- Va trasferito come **sottosezione del capitolo Progettazione** (o del nuovo
  capitolo sugli agenti AI), non lasciato come capitolo orfano.
- Aggiungere lista concreta degli strumenti MCP usati (dal server `grafana`):
  `search_dashboards`, `update_dashboard`, `get_dashboard_by_uid`,
  `list_prometheus_metric_names`, `list_prometheus_label_values`,
  `query_prometheus`, `list_loki_label_values`, `query_loki_logs`.
- Menzionare la **regola di validazione**: dopo ogni modifica dashboard,
  verifica con MCP che metriche e label usati esistano davvero in Prometheus
  (regola presente nell'agente `sia-telemetry-developer`).
- Chiudere la sezione con "Retention dati telemetria" (oggi è una riga
  troncata): documenta i tempi di retention scelti per Tempo/Loki/Prometheus
  e le motivazioni (costo storage vs finestra di indagine utile).

## Glossario e bibliografia

- Voci glossario da aggiungere/rivedere: MELT, CNCF, OTLP, LGTM, exemplar,
  baggage, sampler, processor, span, trace, metric, Counter, Histogram,
  UpDownCounter, ObservableGauge, percentile, p50/p95/p99, throughput,
  cardinality, enduser, MCP, Claude Code, skill.
- Spostare RFID a piè di pagina (come da TODO già presente).
- Bibliografia: aggiungere:
  - specifica OpenTelemetry (opentelemetry.io/docs/specs/otel/),
  - OTEP exemplars (opentelemetry.io/docs/specs/otel/metrics/data-model/#exemplars),
  - documentazione Serilog Sinks OpenTelemetry,
  - Grafana Tempo / Loki / Mimir docs,
  - "Design Patterns: Elements of Reusable Object-Oriented Software" (GoF)
    per Adapter e Factory,
  - Model Context Protocol spec (modelcontextprotocol.io).

---

# Nuovi capitoli richiesti

Di seguito una proposta di **posizionamento** e **contenuto** per i quattro
capitoli/sezioni aggiuntive richieste. Ognuno è scritto in forma di outline
pronto da convertire in `.tex`.

## 1. Nuovo capitolo — *Ottimizzazione dei tempi tramite agenti AI*

**Posizionamento consigliato**: capitolo autonomo tra "Tecnologie adottate" e
"Progettazione", oppure sottosezione finale di "Codifica". Preferibile
come capitolo a sé perché trasversale all'intero stage.

**Titolo proposto**: "Metodologia assistita da agenti AI".

**Outline**:

1. **Contesto e motivazione**
   - Durata stage limitata (300 h / 8 settimane) e ampiezza del tema
     (full-stack .NET + Angular + osservabilità + dashboard).
   - Scelta di adottare *Claude Code* come agente di sviluppo affiancato, non
     come generatore one-shot.
2. **Ambiti di utilizzo degli agenti AI nello stage**
   - **Studio delle tecnologie**: OpenTelemetry SDK .NET, Tempo/Loki/
     Prometheus, PromQL/TraceQL/LogQL, convenzioni Serilog OTLP. L'agente
     ha sintetizzato documentazione e esempi in poche ore, riducendo il tempo
     di onboarding stimato dalla prima pianificazione.
   - **Prove di implementazione** (PoC): il generatore ha permesso di
     scartare alternative (es. sampler head-based vs tail-based, export
     diretto vs via Collector) producendo rapidamente codice compilabile da
     validare in locale.
   - **Propagazione della telemetria** agli altri moduli del framework
     (Auth, Base, Grid, Crud, …) tramite **skill dedicate**:
     `sia-telemetry-setup`, `sia-telemetry-traces`, `sia-telemetry-metrics`,
     `sia-telemetry-logs`, `sia-telemetry-grafana`. Ogni skill è un contratto
     procedurale che garantisce convenzioni uniformi (naming, cardinalità,
     attributi resource) in ogni modulo, anche quando instrumentato da
     persone diverse.
3. **Workflow adottato** (Plan → Confirm → Implement)
   - L'agente produce `plan.md` e `task.md` in
     `.claude/implementations/{yy}/{MM}/{dd}_{titolo}/`.
   - Lo sviluppatore revisiona e approva prima dell'esecuzione.
   - Ogni implementazione chiude con una dashboard Grafana versionata in
     `Diagnostics/{nome}.dashboard.json` e validata contro Prometheus.
4. **Confronto pianificazione vs esecuzione**
   - Tabella: per ogni fase, ore preventivate / ore effettive / risparmio.
   - Commento qualitativo: dove l'agente ha accelerato (studio, PoC,
     replicazione di pattern) e dove no (decisioni architetturali, analisi di
     cardinalità, revisione semantica).
5. **Limiti e mitigazioni**
   - Allucinazioni su metriche/label inesistenti → regola *validazione con
     MCP Grafana* prima di chiudere il task.
   - Overfitting del prompt al primo modulo instrumentato → skill riscritte
     in termini generali, non riferite a un modulo specifico.
   - Perdita di contesto su sessioni lunghe → memorizzazione persistente
     (`MEMORY.md` per progetto e per agente, `plan.md`/`task.md` per task).
6. **Conclusioni della sezione**: l'uso di agenti AI come "pair engineer"
   strutturato, non come strumento di autocompletamento, ha reso fattibile in
   8 settimane sia il lavoro di instrumentazione sia la propagazione
   trasversale a più moduli.

## 2. Nuova sezione — *Design pattern applicati al progetto*

**Posizionamento**: sottosezione di "Progettazione" (subito dopo "Scelte
architetturali") oppure nuovo capitolo "Scelte progettuali e design pattern".

### 2.1 Pattern *Adapter*

**Dove**: classi `Sia{Modulo}Diagnostics` che espongono
`public static readonly IControllerDiagnostics AsControllerDiagnostics`.

**Problema risolto**: il framework fornisce un metodo condiviso
`BaseApiController.RecordMetricError(IControllerDiagnostics, Exception, string)`
che incapsula la logica di unwrap di `SiaLoggerException.OriginalException` e
l'applicazione dei tag `exception.type` e `sia.operation`. Ogni modulo con
controller deve riusarlo, ma ha il proprio `Counter<long> ControllerErrors`
registrato sul `Meter` del modulo (naming `sia.{modulo}.controller.errors`).

**Soluzione**: per ciascun modulo viene definita una classe annidata
`private sealed ControllerDiagnosticsAdapter : IControllerDiagnostics` che
adatta il counter statico del modulo al contratto comune
`IControllerDiagnostics`. Il controller chiama sempre
`RecordMetricError(Sia{Modulo}Diagnostics.AsControllerDiagnostics, …)` senza
conoscere l'implementazione concreta.

**Vantaggi**:
- contratto unico a livello framework, counter distinti per modulo
  (cardinalità controllata);
- logica di unwrap/tag scritta una sola volta;
- zero ripetizione nel controller (ogni `catch` è una riga).

**Esempio**: riferimento canonico `Sia.Devextreme.Crud`
(`SiaDevextremeCrudDiagnostics.AsControllerDiagnostics`, `CrudDataController`).

### 2.2 Pattern *Factory*

**Dove** (indicativo — verificare i nomi effettivi nel codice):

- Costruzione del `Sampler` combinato (per-source + globale) in
  `BaseApplicationBuilder`: il sampler finale è prodotto da un metodo
  factory che, letti i valori di `SamplingRatio` globale e i ratio per-source
  da `appsettings`, restituisce l'istanza corretta di
  `SiaParentBasedSampler` / `GlobalRatioParentBasedSampler`.
- Registrazione dei `Meter`/`ActivitySource`: le classi `Sia{Modulo}Diagnostics`
  agiscono come *Static Factory* centralizzata per gli strumenti di
  osservabilità del modulo (Counter, Histogram, UpDownCounter), così che il
  resto del codice applicativo non istanzi mai direttamente gli strumenti OTel.
- Creazione dei `ResourceBuilder` (attributi `service.name`,
  `sia.product_type`, `sia.customer_name`) tramite un metodo factory che
  legge la configurazione e produce un `Resource` consistente per tutti e
  tre i segnali (trace/metric/log).

**Vantaggi**:
- la configurazione da `appsettings` viene tradotta in oggetti OTel in un
  solo punto;
- i moduli vedono solo istanze pronte all'uso, senza conoscere il pipeline;
- semplifica i test (si può sostituire la factory con una variante no-op).

### 2.3 (Opzionale) *Decorator* e *Observer* già presenti

Se ti interessa ampliare, accenna anche a:
- **Decorator**: `BaggageActivityProcessor` decora ogni `Activity` creata con
  i tag `enduser.*` letti dal Baggage OpenTelemetry.
- **Observer / Publish-Subscribe**: l'intero modello OTel è un
  producer/consumer (ActivitySource/Meter producono eventi, Exporter li
  consumano) — vale la pena citarlo come "pattern implicito" dello standard.

## 3. Nuova sezione — *Tracciamento dell'identità utente (middleware + Baggage)*

**Posizionamento**: sottosezione di "Progettazione > Modello delle trace" o
sottosezione dedicata "Arricchimento del contesto utente".

**Outline**:

1. **Obiettivo**
   - Ogni trace generata da una chiamata autenticata deve riportare chi l'ha
     prodotta (`enduser.id`, `enduser.name`), senza modificare i moduli
     applicativi.
2. **Meccanismo**
   - Middleware ASP.NET Core `Application.Shared.Telemetry.EnrichActivityWithUserMiddleware`,
     registrato in `BaseApplicationBuilder` **dopo** `UseAuthentication()`.
   - Il middleware legge le claim `userId` e `userName` da `HttpContext.User`
     e le pubblica nel `Baggage` di OpenTelemetry
     (`Baggage.Current.SetBaggage("enduser.id", …)`).
   - Un `ActivityProcessor` custom (`BaggageActivityProcessor`) intercetta
     l'hook `OnStart` di ogni nuova `Activity` e copia ogni entry di baggage
     con prefisso `enduser.` come tag sull'Activity.
3. **Conseguenze**
   - **Copertura automatica**: span di ASP.NET Core, SqlClient, Npgsql, e di
     tutti i moduli Sia (`Sia.Base`, `Sia.Auth`, `Sia.Grid`, `Sia.Crud`, …)
     ricevono `enduser.id` ed `enduser.name` senza una riga di codice
     aggiunta nei moduli.
   - **Child span**: il baggage viene ereditato, quindi le child span
     (es. query SQL generate dentro una request) portano lo stesso
     `enduser.*` della root.
4. **Limiti ed eccezioni**
   - Richieste **non autenticate** (login, health check): il middleware non
     pubblica baggage, le span non hanno `enduser.*` (comportamento
     desiderato).
   - Operazioni **fuori request** (hosted service, background job):
     l'identità utente non esiste. Se necessario, va popolata manualmente
     (es. "system").
5. **Sulle metriche: volutamente NO**
   - `enduser.id` ed `enduser.name` **non vengono mai** aggiunti come tag di
     `Counter`/`Histogram`: esploderebbero la cardinalità di Prometheus (una
     serie temporale per utente × altri tag). La correlazione metrica→utente
     passa invece per gli **exemplars** (sezione successiva): ogni campione
     di istogramma porta il `TraceId` della richiesta, e la trace ha
     `enduser.*`.
6. **Uso nelle dashboard Grafana**
   - Ogni dashboard di modulo deve esporre due variabili template
     `enduser_id` ed `enduser_name` (datasource Tempo, `type: query`,
     `includeAll: true`, `multi: true`).
   - Le variabili si usano **solo nei pannelli TraceQL**, es.:
     `{resource.service.name =~ "${service_name:regex}" && name =~ "sia.xxx.*" && span.enduser.id =~ "${enduser_id:regex}"}`.
   - Non vanno usate in PromQL (label inesistente) né in LogQL (a meno che
     i log non portino effettivamente quei label).

## 4. Nuova sezione — *Exemplars: puntatori metrica → trace*

**Posizionamento**: sottosezione di "Progettazione > Modello delle metriche",
subito dopo la descrizione di Counter/Histogram, oppure sezione a sé
"Correlazione metrica→trace tramite exemplars".

**Outline**:

1. **Definizione**
   - Un *exemplar* è un campione concreto allegato al dato aggregato di una
     metrica: oltre al valore numerico, porta con sé il `TraceId` (e
     opzionalmente il `SpanId`) della richiesta che ha prodotto quel
     campione.
   - È una feature standard di OpenTelemetry (spec "Metrics Data Model",
     sezione *Exemplars*) e supportata nativamente da Prometheus (scrape
     OTLP o remote-write) e Grafana (gutter a fianco dei grafici).
2. **Motivazione**
   - Le metriche aggregate risponderebbero "quanto" e "quando", ma non
     "quale richiesta". Gli exemplars colmano il gap: da un picco in
     `sia_base_query_duration_milliseconds_bucket` si clicca sull'exemplar
     e si salta alla trace corrispondente in Tempo.
   - Essenziale per diagnosticare *tail latency* (p99) senza campionare al
     100% tutte le trace (costoso in storage).
3. **Configurazione nel progetto**
   - Nel pipeline Meter dentro `BaseApplicationBuilder` è impostato
     `SetExemplarFilter(ExemplarFilterType.TraceBased)`: OTel allega un
     exemplar solo quando esiste una trace *sampled* nel contesto del
     campione → zero overhead sulle richieste non campionate.
   - Il `MetricReader` periodico esporta via OTLP exemplars e sample; il
     Collector li inoltra a Prometheus tramite l'exporter OTLP receiver.
   - Prometheus deve avere `--enable-feature=exemplar-storage` (o la
     feature equivalente in Mimir).
   - Grafana: nel datasource Prometheus va abilitata la voce "Exemplars" con
     link alla datasource Tempo (`linkedFields: TraceID`).
4. **Cosa vedo in dashboard**
   - Sui pannelli istogramma percentili (`histogram_quantile`) compaiono
     piccoli diamanti colorati in corrispondenza del campione. Click →
     apertura della trace in Tempo con lo span di origine evidenziato.
5. **Perché abilita la "regola enduser sulle metriche NO"**
   - Non serve mettere `enduser.*` come tag di metrica per rispondere a
     "chi ha generato la richiesta lenta": lo si scopre saltando
     dall'exemplar alla trace, che porta già `enduser.id` ed `enduser.name`
     grazie al middleware (sezione precedente).
6. **Limiti**
   - Non tutti i backend supportano exemplars in ogni tipo di metrica
     (principalmente Histogram; su Counter è meno utile).
   - L'exemplar punta a una trace che deve esistere: se la trace è stata
     scartata dal sampler, il puntatore diventa "orfano". Per questo il
     filtro scelto è `TraceBased`.
7. **Diagramma** (placeholder figura)
   - Flusso: `Counter/Histogram.Record(value, tags)` → pipeline OTel con
     `ExemplarFilterType.TraceBased` → OTLP → Prometheus (storage exemplar)
     → Grafana (click sull'exemplar) → Tempo (span con `enduser.*`).

---

# Ordine suggerito di inserimento dei nuovi contenuti

1. Nel cap. 2 ("Requisiti"): aggiungi RF9/RF10 (enduser + exemplars) e
   rivedi RNF per la cardinalità.
2. Nel cap. 4 ("Tecnologie"): integra criteri di scelta + tabella pro/contro.
3. Nel cap. 5 ("Progettazione"):
   - aggiungi sottosezione "Arricchimento del contesto utente" (punto 3);
   - aggiungi sottosezione "Exemplars per la correlazione metrica→trace"
     (punto 4);
   - aggiungi sottosezione "Design pattern applicati" (punto 2),
     eventualmente come capitolo a sé se lo preferisci.
4. Nuovo capitolo autonomo "Metodologia assistita da agenti AI" (punto 1),
   posizionato prima o dopo "Progettazione" a seconda del flusso narrativo.
5. Ripulisci e sposta il capitolo "Roba da organizzare" (MCP + retention)
   come sottosezioni di "Progettazione" e "Conclusioni".
