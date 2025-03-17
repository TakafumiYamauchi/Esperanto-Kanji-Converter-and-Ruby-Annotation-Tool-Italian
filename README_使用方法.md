# Manuale Utente: Strumento di sostituzione di caratteri (kanji) per il testo in esperanto

## Indice
1. Introduzione
2. Panoramica dell'applicazione
3. Pagina principale: sostituzione del testo
4. Pagina secondaria: generazione di file JSON personalizzati
5. Opzioni di formattazione avanzate
6. Funzionalità speciali e suggerimenti
7. Risoluzione dei problemi comuni

## 1. Introduzione

Benvenuti all'applicazione "Strumento di sostituzione di caratteri (kanji) per il testo in esperanto". Questa applicazione web permette di trasformare testi scritti in esperanto sostituendo le parole con caratteri kanji o annotazioni in altre lingue (in questo caso in italiano), rendendo il testo più comprensibile per chi sta imparando la lingua o desidera visualizzare le connessioni tra l'esperanto e altre lingue.

L'applicazione offre diverse opzioni di visualizzazione:
- Annotazioni HTML (ruby) che mostrano la traduzione sopra il testo originale
- Formato con parentesi
- Semplice sostituzione del testo

Questo manuale vi guiderà attraverso tutte le funzionalità dell'applicazione, spiegando in dettaglio come utilizzarla al meglio.

## 2. Panoramica dell'applicazione

L'applicazione è composta da due pagine principali:

1. **Pagina principale (Sostituzione del testo)**: Permette di inserire testo in esperanto e visualizzare il risultato della sostituzione con caratteri kanji o annotazioni in italiano.

2. **Pagina secondaria (Generazione di file JSON)**: Consente di creare file JSON personalizzati che definiscono le regole di sostituzione da utilizzare nella pagina principale.

Entrambe le pagine sono accessibili dal menu di navigazione laterale di Streamlit.

## 3. Pagina principale: sostituzione del testo

### 3.1 Caricamento del file JSON

Il primo passo è caricare il file JSON contenente le regole di sostituzione. Avete due opzioni:

- **Usa il file JSON predefinito**: L'applicazione utilizza un file JSON predefinito già configurato.
- **Carica un file**: Potete caricare un vostro file JSON personalizzato (creato nella pagina secondaria o modificato manualmente).

Se desiderate scaricare un esempio di file JSON per studiarne la struttura, potete farlo espandendo la sezione "Scarica un file JSON di esempio".

### 3.2 Parametri avanzati

Nella sezione "Parametri avanzati", potete configurare l'elaborazione parallela:

- **Usa l'elaborazione parallela**: Attivate questa opzione per migliorare le prestazioni con testi molto lunghi.
- **Numero di processi simultanei**: Impostate il numero di processi in parallelo (da 2 a 4).

### 3.3 Selezione del formato di output

Scegliete il formato di output desiderato dal menu a tendina:

- **Formato HTML con annotazioni (ruby) e regolazione delle dimensioni**: Mostra il testo originale con annotazioni sopra, regolando automaticamente le dimensioni per una migliore leggibilità.
- **Formato HTML con annotazioni (ruby), regolazione delle dimensioni e sostituzione dei kanji**: Come sopra, ma inverte il testo originale con i kanji.
- **Formato HTML**: Formato HTML base senza regolazione delle dimensioni.
- **Formato HTML con sostituzione dei kanji**: Come sopra, ma inverte il testo originale con i kanji.
- **Formato con parentesi**: Mostra il testo originale seguito dalla traduzione tra parentesi.
- **Formato con parentesi e sostituzione dei kanji**: Mostra i kanji seguiti dal testo originale tra parentesi.
- **Mantieni solo il testo sostituito**: Mostra solo la traduzione, senza il testo originale.

### 3.4 Inserimento del testo

Potete inserire il testo in esperanto in due modi:

- **Inserimento manuale**: Digitate direttamente il testo nell'area di testo.
- **Carica un file**: Caricate un file di testo (UTF-8) contenente il testo in esperanto.

### 3.5 Opzioni per i caratteri speciali dell'esperanto

Scegliete come visualizzare i caratteri speciali dell'esperanto nel risultato:

- **Accento sulla lettera (ĉ → c + ˆ)**: Utilizza il formato con circonflesso (ĉ, ĝ, ĥ, ĵ, ŝ, ŭ).
- **Formato con x (ĉ → cx)**: Utilizza il formato con x (cx, gx, hx, jx, sx, ux).
- **Formato con ^ (ĉ → c^)**: Utilizza il formato con ^ (c^, g^, h^, j^, s^, u^).

### 3.6 Marcatori speciali nel testo

Potete utilizzare marcatori speciali nel testo per controllare la sostituzione:

- **%...%**: Il testo tra simboli % non verrà sostituito e rimarrà invariato.
  Esempio: `La %kongreso% okazas en julio` → La kongreso si svolge a luglio.

- **@...@**: Il testo tra simboli @ verrà sostituito in modo localizzato all'interno di quel frammento.
  Esempio: `La @kongreso@ okazas en julio` → La [congresso con annotazione] si svolge a luglio.

### 3.7 Elaborazione e visualizzazione dei risultati

Dopo aver inserito il testo e configurato le opzioni, cliccate sul pulsante "Invia" per elaborare il testo. Il risultato verrà mostrato sotto il form.

Se il formato scelto include HTML, vedrete due schede:
- **Anteprima HTML**: Mostra il risultato renderizzato.
- **Risultato (codice HTML)**: Mostra il codice HTML generato.

Per altri formati, vedrete solo il testo risultante.

Potete scaricare il risultato cliccando sul pulsante "Scarica il risultato".

## 4. Pagina secondaria: generazione di file JSON personalizzati

La pagina secondaria "Strumento per generare file JSON per la sostituzione di testo (caratteri cinesi) in Esperanto" permette di creare file JSON personalizzati.

### 4.1 Panoramica

Il processo di generazione di un file JSON personalizzato consiste in tre fasi:

1. Preparare il file CSV contenente le corrispondenze tra radici esperanto e traduzioni
2. Preparare il/i file JSON contenenti le regole di scomposizione
3. Configurare l'elaborazione in parallelo (opzionale)

### 4.2 File di esempio

Nella sezione "Elenco di file di esempio (download)" potete scaricare vari file di esempio:

- **CSV di esempio**: Tabelle di corrispondenza tra radici esperanto e traduzioni/kanji
- **JSON di esempio**: File con regole di scomposizione delle radici e configurazioni personali
- **Excel di esempio**: File con radici esperanto e traduzioni in diverse lingue

### 4.3 Formato di output

Selezionate il formato di output desiderato, che sarà utilizzato nel file JSON generato. L'applicazione mostrerà un'anteprima del formato scelto.

### 4.4 Fase 1: Preparare il file CSV

Nella sezione "Fase 1", scegliete come procedere con il file CSV:

- **Carica un file CSV**: Caricate un vostro file CSV contenente le corrispondenze.
- **Usa il file di default**: Utilizzate il file CSV predefinito.

Il formato del CSV dovrebbe avere almeno due colonne: la prima con le radici in esperanto, la seconda con le traduzioni o i kanji.

### 4.5 Fase 2: Preparare i file JSON

Nella sezione "Fase 2", configurate i file JSON che definiscono le regole di scomposizione:

1. **File JSON per definire la scomposizione delle radici esperanto**:
   - Carica un file JSON
   - Usa il file di default

2. **File JSON per definire il testo sostituito**:
   - Carica un file JSON
   - Usa il file di default

### 4.6 Fase 3: Impostazioni avanzate

Come nella pagina principale, potete configurare l'elaborazione parallela per migliorare le prestazioni durante la creazione del file JSON.

### 4.7 Generazione del file JSON

Cliccate sul pulsante "Crea il file JSON per la sostituzione" per generare il file JSON. Il processo potrebbe richiedere alcuni secondi, soprattutto per file grandi.

Una volta completato, potrete scaricare il file JSON generato cliccando sul pulsante "Scarica la lista definitiva di sostituzione".

## 5. Opzioni di formattazione avanzate

### 5.1 Formato HTML con annotazioni ruby

Il formato HTML con annotazioni ruby è particolarmente utile per l'apprendimento linguistico. Le annotazioni ruby mostrano la traduzione o i kanji sopra il testo originale.

L'applicazione offre diverse opzioni per personalizzare la visualizzazione delle annotazioni ruby:

- **Regolazione automatica delle dimensioni**: L'applicazione regola automaticamente le dimensioni delle annotazioni in base alla lunghezza del testo.
- **Inversione testo/kanji**: Potete scegliere se mostrare il testo originale con le annotazioni sopra, o i kanji con il testo originale come annotazione.

### 5.2 Esempio di output

Ecco un esempio di come appare il testo con annotazioni ruby:

```
La <ruby>homo<rt>uomo</rt></ruby> <ruby>parolas<rt>parla</rt></ruby> <ruby>esperante<rt>in esperanto</rt></ruby>.
```

Visualizzato nel browser, apparirà con "uomo", "parla" e "in esperanto" come annotazioni sopra le rispettive parole in esperanto.

## 6. Funzionalità speciali e suggerimenti

### 6.1 Efficienza con testi lunghi

Per testi molto lunghi, si consiglia di:

- Attivare l'elaborazione parallela
- Aumentare il numero di processi simultanei (se il vostro computer ha più core)
- Considerare di dividere il testo in parti più piccole se l'elaborazione è ancora troppo lenta

### 6.2 Personalizzazione delle sostituzioni

Per personalizzare le sostituzioni:

1. Create un file CSV con le vostre corrispondenze (radice esperanto → traduzione)
2. Utilizzate la pagina secondaria per generare un file JSON personalizzato
3. Caricate questo file JSON personalizzato nella pagina principale

### 6.3 Utilizzo dei marcatori speciali

I marcatori speciali `%...%` e `@...@` sono particolarmente utili per:

- **%...%**: Mantenere invariati nomi propri, termini tecnici o parole che non desiderate tradurre
- **@...@**: Applicare una sostituzione specifica solo a determinate occorrenze di una parola

## 7. Risoluzione dei problemi comuni

### 7.1 Problemi di caricamento dei file

- **Il file CSV non viene caricato**: Assicuratevi che il file sia nel formato UTF-8 e abbia almeno due colonne.
- **Il file JSON non viene caricato**: Verificate che il formato JSON sia valido e contenga i campi richiesti.

### 7.2 Problemi di visualizzazione

- **Le annotazioni ruby non appaiono correttamente**: Alcuni browser o versioni potrebbero non supportare completamente le annotazioni ruby. Provate con un browser più recente.
- **I caratteri speciali dell'esperanto non vengono visualizzati**: Assicuratevi che il vostro browser supporti i caratteri Unicode e che il file di input sia codificato in UTF-8.

### 7.3 Problemi di prestazioni

- **L'elaborazione è troppo lenta**: Attivate l'elaborazione parallela e aumentate il numero di processi. Se il problema persiste, provate a dividere il testo in parti più piccole.
- **L'applicazione si blocca**: Potrebbe essere dovuto a un testo troppo lungo o a un file JSON troppo complesso. Provate a ridurre la dimensione del testo o a semplificare il file JSON.

---

## Conclusione

Questo strumento offre un modo innovativo per lavorare con testi in esperanto, facilitando l'apprendimento e la comprensione attraverso sostituzioni visive con kanji o traduzioni in italiano. Speriamo che questa guida vi aiuti a utilizzare al meglio tutte le funzionalità dell'applicazione.

Per ulteriori informazioni o supporto, consultate i link forniti nella parte inferiore della pagina principale, dove troverete versioni dell'applicazione in altre lingue e documentazione aggiuntiva.
