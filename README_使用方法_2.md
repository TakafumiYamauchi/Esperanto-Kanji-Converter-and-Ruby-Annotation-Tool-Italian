# Manuale d'Uso: Strumento di Sostituzione di Testo in Esperanto

## Introduzione

Benvenuti a questo strumento di sostituzione di testo in esperanto con caratteri kanji o annotazioni HTML. Questa applicazione vi permette di convertire testi scritti in esperanto utilizzando diversi formati di visualizzazione, incluse annotazioni ruby in HTML o sostituzione con caratteri kanji, facilitando così l'apprendimento e la comprensione della lingua esperanto.

## Indice dei Contenuti

1. Panoramica dell'applicazione
2. Pagina principale: Conversione del testo
3. Pagina secondaria: Generazione di file JSON per la sostituzione
4. Formati di output disponibili
5. Funzionalità avanzate
6. Esempi di utilizzo
7. Domande frequenti

## 1. Panoramica dell'Applicazione

Questa applicazione web è composta da due pagine principali:

- **Pagina principale**: Permette di convertire direttamente il testo in esperanto utilizzando file JSON predefiniti o personalizzati.
- **Pagina di generazione JSON**: Consente di creare file JSON personalizzati per definire come le radici in esperanto devono essere sostituite o annotate.

L'applicazione supporta diverse modalità di visualizzazione, dall'aggiunta di annotazioni ruby in HTML alla sostituzione completa con caratteri kanji, e offre funzionalità avanzate come l'elaborazione parallela per migliorare le prestazioni con testi di grandi dimensioni.

## 2. Pagina Principale: Conversione del Testo

### Passo 1: Gestione del File JSON

Nella parte superiore della pagina, è necessario selezionare come gestire il file JSON che contiene le regole di sostituzione:

- **Usa il file JSON predefinito**: Utilizza le regole di sostituzione integrate nell'applicazione.
- **Carica un file**: Permette di caricare un file JSON personalizzato con le proprie regole di sostituzione.

> **Nota**: È possibile scaricare un file JSON di esempio espandendo la sezione "Scarica un file JSON di esempio (per la sostituzione)".

### Passo 2: Impostazioni Avanzate (Opzionale)

Nella sezione "Parametri avanzati", è possibile attivare l'elaborazione parallela per migliorare le prestazioni con testi lunghi:

- **Usa l'elaborazione parallela**: Attiva/disattiva l'elaborazione multiprocesso.
- **Numero di processi simultanei**: Imposta quanti processi utilizzare (consigliato: lasciare il valore predefinito di 4).

### Passo 3: Selezione del Formato di Output

Selezionate il formato desiderato per il risultato finale:

- **Formato HTML con annotazioni (ruby) e regolazione delle dimensioni**: Aggiunge annotazioni HTML sopra il testo originale con dimensione adattiva.
- **Formato HTML con annotazioni (ruby), regolazione delle dimensioni e sostituzione dei kanji**: Sostituisce il testo con kanji e mostra il testo originale in esperanto come annotazione.
- **Formato HTML**: Versione base del formato HTML.
- **Formato HTML con sostituzione dei kanji**: Versione base con sostituzione kanji.
- **Formato con parentesi**: Mostra il testo originale seguito dalla traduzione/kanji tra parentesi.
- **Formato con parentesi e sostituzione dei kanji**: Inverso del precedente.
- **Mantieni solo il testo sostituito**: Sostituisce completamente il testo originale.

### Passo 4: Inserimento del Testo

Scegliete la fonte del testo in esperanto:

- **Inserimento manuale**: Digitare direttamente il testo nell'area dedicata.
- **Carica un file**: Caricare un file di testo (UTF-8).

### Passo 5: Opzioni di Visualizzazione dei Caratteri Speciali

Selezionate come visualizzare i caratteri speciali dell'esperanto:

- **Accento sulla lettera (ĉ → c + ˆ)**: Utilizza i caratteri con accento circonflesso.
- **Formato con x (ĉ → cx)**: Utilizza la notazione con "x" (comune in contesti digitali).
- **Formato con ^ (ĉ → c^)**: Utilizza il simbolo "^" dopo la lettera.

### Passo 6: Elaborazione

1. Inserite il testo in esperanto nell'area di testo.
2. Fate clic su "Invia" per avviare l'elaborazione.
3. Il risultato verrà visualizzato nella parte inferiore della pagina.

> **Suggerimento**: Potete racchiudere parti del testo tra simboli **%** (es. `%testo da non sostituire%`) per evitare che vengano sostituite, oppure tra simboli **@** (es. `@testo da sostituire localmente@`) per applicare sostituzioni specifiche solo a quella porzione.

### Passo 7: Risultato e Download

Il risultato dell'elaborazione viene mostrato in diverse schede:

- **Anteprima HTML** (se applicabile): Mostra come appare il testo con le annotazioni.
- **Risultato (codice HTML)** o **Testo risultante**: Mostra il codice HTML generato o il testo elaborato.

Utilizzate il pulsante "Scarica il risultato" per salvare il risultato come file.

## 3. Pagina Secondaria: Generazione di File JSON per la Sostituzione

Questa pagina permette di creare file JSON personalizzati per la sostituzione del testo in esperanto.

### Passo 1: Preparazione del File CSV

Il file CSV deve contenere le corrispondenze tra radici esperanto e traduzioni o caratteri kanji:

- **Carica un file CSV**: Caricate un file CSV personalizzato.
- **Usa il file di default**: Utilizzate il file CSV predefinito.

> **Nota**: Potete scaricare esempi di file CSV dalla sezione "Elenco di file di esempio (download)".

### Passo 2: Preparazione dei File JSON

Sono necessari due file JSON:

1. **JSON per la scomposizione delle radici esperanto**: Definisce come scomporre le parole esperanto in radici.
2. **JSON per il testo sostituito**: Definisce impostazioni personalizzate sul testo da sostituire.

Per entrambi, è possibile caricare un file personalizzato o utilizzare i file predefiniti.

### Passo 3: Impostazioni Avanzate (Opzionale)

Come nella pagina principale, è possibile attivare l'elaborazione parallela per migliorare le prestazioni.

### Passo 4: Generazione del File JSON

Fate clic su "Crea il file JSON per la sostituzione" per generare il file JSON combinato. Il processo potrebbe richiedere alcuni secondi, specialmente con molte radici esperanto.

Una volta completato, potrete scaricare il file JSON finale utilizzando il pulsante "Scarica la lista definitiva di sostituzione".

## 4. Formati di Output Disponibili

### Formato HTML con annotazioni ruby

Questo formato aggiunge annotazioni sopra il testo originale in esperanto:

```html
<ruby>esperanto<rt>traduzione/kanji</rt></ruby>
```

L'opzione con "regolazione delle dimensioni" adatta automaticamente la dimensione dell'annotazione in base alla lunghezza del testo.

### Formato con parentesi

Mostra il testo originale seguito dalla traduzione tra parentesi:

```
esperanto(traduzione/kanji)
```

O viceversa con l'opzione "sostituzione dei kanji":

```
traduzione/kanji(esperanto)
```

### Sostituzione semplice

Sostituisce completamente il testo originale con la traduzione o i caratteri kanji, senza mantenere il testo originale.

## 5. Funzionalità Avanzate

### Marcatori Speciali nel Testo

- **%testo%**: Il testo tra simboli % non verrà sostituito e rimarrà invariato.
- **@testo@**: Il testo tra simboli @ verrà sostituito in modo localizzato, applicando regole specifiche solo a quella porzione.

### Elaborazione Parallela

Per testi lunghi, l'elaborazione parallela può accelerare significativamente il processo di sostituzione. È consigliabile attivarla per file di grandi dimensioni.

### Personalizzazione delle Regole di Sostituzione

Utilizzando la pagina di generazione JSON, è possibile definire regole personalizzate per:

- Come scomporre le parole esperanto in radici
- Come gestire suffissi verbali e altre forme grammaticali
- Definire traduzioni o caratteri kanji specifici per determinate radici

## 6. Esempi di Utilizzo

### Esempio 1: Conversione base di un testo

1. Nella pagina principale, selezionate "Usa il file JSON predefinito"
2. Selezionate "Formato HTML con annotazioni (ruby) e regolazione delle dimensioni"
3. Inserite il testo: `La rapida vulpo saltas super la pigra hundo.`
4. Selezionate "Accento sulla lettera"
5. Fate clic su "Invia"

### Esempio 2: Utilizzo dei marcatori speciali

Inserite il testo: `Mi %ne% komprenas @la lingvon@ esperantan.`

In questo esempio:
- `%ne%` rimarrà invariato (non verrà sostituito)
- `@la lingvon@` verrà sostituito localmente
- Il resto del testo sarà elaborato normalmente

### Esempio 3: Creazione di un file JSON personalizzato

1. Nella pagina di generazione JSON, caricate un file CSV con le vostre corrispondenze
2. Utilizzate i file JSON predefiniti per la scomposizione delle radici
3. Fate clic su "Crea il file JSON per la sostituzione"
4. Utilizzate il file JSON generato nella pagina principale per le vostre sostituzioni personalizzate

## 7. Domande Frequenti

### Come posso evitare che certe parti del testo vengano sostituite?
Racchiudetele tra simboli % (es. `%testo da non sostituire%`).

### Posso definire traduzioni personalizzate per le radici esperanto?
Sì, create un file CSV con le vostre corrispondenze e utilizzate la pagina di generazione JSON per creare un file JSON personalizzato.

### L'applicazione funziona offline?
No, questa è un'applicazione web basata su Streamlit Cloud e richiede una connessione internet.

### Posso usare l'applicazione per altre lingue oltre all'esperanto?
L'applicazione è specificamente progettata per l'esperanto, ma i principi potrebbero essere adattati ad altre lingue modificando i file di configurazione.

### Come posso ottimizzare l'applicazione per testi molto lunghi?
Attivate l'elaborazione parallela nelle impostazioni avanzate e aumentate il numero di processi simultanei.

---

Speriamo che questo manuale vi aiuti a utilizzare efficacemente lo strumento di sostituzione di testo in esperanto. Per ulteriori informazioni o supporto, visitate il repository GitHub dell'applicazione indicato nella sezione dei link alla fine della pagina principale.

Buon lavoro con le vostre traduzioni e annotazioni in esperanto!