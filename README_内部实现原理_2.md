# Guida Tecnica per Programmatori: Strumento di Sostituzione di Caratteri (Kanji) per il Testo in Esperanto

## Indice
1. Architettura dell'Applicazione
2. Flusso di Dati e Processi
3. Moduli e Componenti Principali
4. Algoritmi di Sostituzione del Testo
5. Elaborazione Parallela
6. Strutture Dati e Formati JSON
7. Tecniche di Ottimizzazione
8. Analisi del Codice per Componenti Specifici

---

## 1. Architettura dell'Applicazione

### 1.1 Struttura dei File

L'applicazione è strutturata in quattro componenti principali:

- **main.py**: Punto di entrata dell'applicazione Streamlit, gestisce l'interfaccia utente principale e coordina la sostituzione del testo.
- **Pagina per generare file JSON per sostituire testi in esperanto con stringhe (kanji).py**: Pagina secondaria di Streamlit per generare file JSON personalizzati.
- **esp_text_replacement_module.py**: Modulo di utilità che contiene funzioni per la sostituzione del testo, la gestione dei placeholder e l'elaborazione parallela.
- **esp_replacement_json_make_module.py**: Modulo di utilità per la creazione di file JSON, la misurazione della larghezza del testo e la formattazione dell'output.

### 1.2 Architettura Modulare

L'applicazione utilizza un'architettura modulare con separazione delle responsabilità:

- **Interfaccia utente**: Gestita da Streamlit nei file main.py e nella pagina secondaria
- **Logica di elaborazione**: Distribuita tra i moduli di utilità
- **Gestione dei dati**: Importazione/esportazione di JSON e CSV, manipolazione dei dati in memoria

### 1.3 Dipendenze Esterne

L'applicazione dipende da diverse librerie Python:
- **Streamlit**: Per l'interfaccia utente web
- **Pandas**: Per la manipolazione dei dati tabulari (CSV)
- **Re (regex)**: Per la manipolazione avanzata delle stringhe
- **Multiprocessing**: Per l'elaborazione parallela
- **JSON**: Per la gestione dei file di configurazione

---

## 2. Flusso di Dati e Processi

### 2.1 Flusso di Dati Principale

Il flusso di dati principale dell'applicazione segue questi passaggi:

1. **Input**: Testo in esperanto (manuale o da file)
2. **Configurazione**: Caricamento del file JSON con le regole di sostituzione
3. **Pre-elaborazione**: Conversione dei caratteri dell'esperanto, identificazione dei placeholder
4. **Elaborazione**: Applicazione delle regole di sostituzione (in parallelo o sequenziale)
5. **Post-elaborazione**: Formattazione dell'output secondo il formato scelto
6. **Output**: Visualizzazione e possibilità di download del risultato

### 2.2 Processo di Generazione JSON

Il processo di generazione dei file JSON personalizzati segue questi passaggi:

1. **Input**: File CSV con coppie di radici esperanto e traduzioni
2. **Configurazione**: Parametri per la scomposizione delle radici e le regole di sostituzione
3. **Elaborazione**: Creazione di liste di sostituzione per diversi contesti (globale, locale, radici di 2 caratteri)
4. **Output**: File JSON combinato contenente tutte le regole di sostituzione

---

## 3. Moduli e Componenti Principali

### 3.1 Componenti del File Main.py

```
main.py
├── Configurazione Streamlit
├── Funzioni di Utilità (@st.cache_data)
│   └── load_replacements_lists
├── Gestione Input Utente
│   ├── Selezione Sorgente JSON
│   ├── Caricamento Placeholder
│   ├── Parametri Avanzati
│   ├── Selezione Formato Output
│   ├── Input Testo (Manuale/File)
│   └── Opzioni Caratteri Speciali
├── Elaborazione Testo
│   ├── Elaborazione Sequenziale (orchestrate_comprehensive_esperanto_text_replacement)
│   └── Elaborazione Parallela (parallel_process)
└── Visualizzazione Risultati
    ├── Anteprima HTML/Testo
    └── Download Risultato
```

La struttura `main.py` implementa l'interfaccia utente Streamlit e il flusso di elaborazione principale. Utilizza il decoratore `@st.cache_data` per memorizzare nella cache i risultati del caricamento dei file JSON, migliorando le prestazioni.

### 3.2 Componenti della Pagina di Generazione JSON

```
Pagina per generare file JSON...
├── Configurazione Streamlit
├── Caricamento Dati Iniziali
│   ├── Importazione Placeholder
│   ├── Caricamento Dizionario Larghezze Caratteri
│   └── Definizione Suffissi/Prefissi
├── Interfaccia Utente
│   ├── Selezione CSV Input
│   ├── Caricamento JSON Impostazioni
│   └── Parametri Avanzati
├── Processo di Generazione JSON
│   ├── Caricamento Dati Radici Esperanto
│   ├── Applicazione Regole CSV
│   ├── Ottimizzazione Priorità di Sostituzione
│   ├── Generazione Regole per Radici di 2 Caratteri
│   ├── Applicazione Impostazioni Personalizzate
│   └── Creazione File JSON Finale
└── Download Risultato
```

Questa pagina implementa un processo più complesso per generare file JSON personalizzati, con particolare attenzione all'ottimizzazione delle priorità di sostituzione.

### 3.3 Moduli di Utilità

I moduli `esp_text_replacement_module.py` e `esp_replacement_json_make_module.py` contengono diverse funzioni di utilità che supportano i processi principali:

- Funzioni di conversione dei caratteri dell'esperanto
- Funzioni di sostituzione sicura con placeholder
- Funzioni per l'elaborazione parallela
- Funzioni di misurazione e formattazione del testo
- Funzioni di manipolazione delle stringhe HTML

---

## 4. Algoritmi di Sostituzione del Testo

### 4.1 Strategia di Sostituzione con Placeholder

L'applicazione utilizza una strategia sofisticata per la sostituzione del testo basata su placeholder per evitare sostituzioni parziali o conflitti:

```
Testo originale → Sostituzione con placeholder → Sostituzione placeholder con testo finale
```

Ad esempio, per sostituire "esperanto" con "世界语":
1. "esperanto" → "%PLACEHOLDER123%"
2. "%PLACEHOLDER123%" → "世界语"

Questo approccio evita problemi quando una parola è contenuta in un'altra (ad es. "esper" in "esperanto").

### 4.2 Gestione di Pattern Speciali

L'applicazione supporta pattern speciali nel testo:

- **%text%**: Parti di testo che non devono essere sostituite
- **@text@**: Parti di testo da sostituire localmente

Questi pattern vengono identificati con espressioni regolari e gestiti separatamente durante il processo di sostituzione.

### 4.3 Algoritmo di Sostituzione Principale

L'algoritmo principale di sostituzione (`orchestrate_comprehensive_esperanto_text_replacement`) segue questi passaggi:

1. Normalizzazione degli spazi e conversione dei caratteri dell'esperanto
2. Sostituzione temporanea delle parti racchiuse tra % (da preservare)
3. Sostituzione localizzata delle parti racchiuse tra @ 
4. Sostituzione globale basata sulle regole nel file JSON
5. Sostituzione delle radici di 2 caratteri (eseguita due volte per gestire sovrapposizioni)
6. Ripristino dei placeholder ai loro valori originali
7. Formattazione finale in base al formato di output scelto

---

## 5. Elaborazione Parallela

### 5.1 Implementazione del Multiprocessing

L'applicazione implementa l'elaborazione parallela per migliorare le prestazioni con testi lunghi:

```python
def parallel_process(text, num_processes, ...):
    # Divisione del testo in segmenti
    lines = re.findall(r'.*?\n|.+$', text)
    
    # Calcolo linee per processo
    lines_per_process = max(num_lines // num_processes, 1)
    
    # Creazione range per ogni processo
    ranges = [(i * lines_per_process, (i + 1) * lines_per_process) 
             for i in range(num_processes)]
    
    # L'ultimo processo elabora il resto delle linee
    ranges[-1] = (ranges[-1][0], num_lines)
    
    # Esecuzione parallela
    with multiprocessing.Pool(processes=num_processes) as pool:
        results = pool.starmap(process_segment, [...])
    
    # Unione risultati
    return ''.join(results)
```

Questa implementazione divide il testo in segmenti basati sulle linee e distribuisce l'elaborazione tra più processi.

### 5.2 Gestione degli Errori di Pickle

Per evitare errori comuni con `multiprocessing` in Streamlit, l'applicazione configura esplicitamente il metodo di avvio:

```python
try:
    multiprocessing.set_start_method("spawn")
except RuntimeError:
    pass  # Il metodo è già impostato
```

Questo evita errori di serializzazione (PicklingError) che potrebbero verificarsi con il metodo di avvio predefinito.

### 5.3 Parallelizzazione nella Generazione JSON

Anche il processo di generazione dei file JSON utilizza l'elaborazione parallela per creare il dizionario di pre-sostituzioni:

```python
def parallel_build_pre_replacements_dict(E_stem_with_Part_Of_Speech_list, replacements, num_processes):
    # Divisione dei dati in chunk
    chunks = [...]
    
    # Elaborazione parallela
    with multiprocessing.Pool(num_processes) as pool:
        partial_dicts = pool.starmap(process_chunk_for_pre_replacements, [...])
    
    # Unione dei risultati
    merged_dict = {}
    for partial_d in partial_dicts:
        # Fusione dei dizionari parziali...
```

---

## 6. Strutture Dati e Formati JSON

### 6.1 Struttura dei File JSON di Sostituzione

Il file JSON di sostituzione contiene tre liste principali:

1. **replacements_final_list**: Lista per sostituzioni globali
   ```json
   [
     ["esperanto", "<ruby>esperanto<rt>世界语</rt></ruby>", "$PLACEHOLDER123$"],
     ["lingvo", "<ruby>lingvo<rt>语言</rt></ruby>", "$PLACEHOLDER456$"],
     ...
   ]
   ```

2. **replacements_list_for_localized_string**: Lista per sostituzioni locali (per @...@)
   ```json
   [
     ["esperanto", "<ruby>esperanto<rt>世界语</rt></ruby>", "@PLACEHOLDER789@"],
     ...
   ]
   ```

3. **replacements_list_for_2char**: Lista per radici di 2 caratteri
   ```json
   [
     ["ar$", "<ruby>ar<rt>集团</rt></ruby>$", "$PLACEHOLDER101$"],
     ...
   ]
   ```

Ogni elemento è una tupla di tre valori: testo originale, testo sostitutivo e placeholder.

### 6.2 Struttura dei File CSV di Input

I file CSV di input contengono coppie di radici esperanto e le loro traduzioni:

```
radice_esperanto,traduzione
am,amore
amik,amico
esper,speranza
...
```

### 6.3 JSON per la Scomposizione delle Radici

Il file JSON per la scomposizione delle radici contiene regole per la manipolazione delle radici esperanto:

```json
[
  ["am", "dflt", ["verbo_s1"]],
  ["esper", "dflt", ["ne"]],
  ...
]
```

Questo formato specifica:
- La radice esperanto
- La priorità (dflt = default, -1 = escludi)
- Lista di modificatori (verbo_s1 = applica suffissi verbali, ne = non aggiungere nulla)

---

## 7. Tecniche di Ottimizzazione

### 7.1 Ottimizzazione delle Priorità di Sostituzione

L'applicazione utilizza un sistema di priorità basato sulla lunghezza del testo per garantire che le parole più lunghe vengano sostituite prima:

```python
# Priorità = lunghezza parola * 10000
priority = len(word) * 10000
```

Questo evita problemi con parole che sono sottostringhe di altre (ad es. "art" in "artikolo").

### 7.2 Caching in Streamlit

L'applicazione utilizza il decoratore `@st.cache_data` per memorizzare i risultati del caricamento dei file JSON:

```python
@st.cache_data
def load_replacements_lists(json_path):
    # Carica il file JSON una sola volta
    with open(json_path, 'r', encoding='utf-8') as f:
        data = json.load(f)
    # Elaborazione dati...
    return (replacements_final_list, replacements_list_for_localized_string, replacements_list_for_2char)
```

Questo migliora significativamente le prestazioni evitando di ricaricare file JSON di grandi dimensioni a ogni interazione.

### 7.3 Ottimizzazione della Sostituzione delle Radici

L'applicazione implementa una logica sofisticata per gestire le radici esperanto e i loro affissi (prefissi, suffissi):

- Le radici vengono ordinate per lunghezza (le più lunghe prima)
- Vengono generati automaticamente termini con suffissi grammaticali (as, is, os, ecc.)
- Vengono gestiti casi speciali come "an" e "on" che possono essere sia suffissi che parti di parole

---

## 8. Analisi del Codice per Componenti Specifici

### 8.1 Funzione di Sostituzione Sicura

La funzione `safe_replace` è un componente critico che implementa la strategia di sostituzione con placeholder:

```python
def safe_replace(text, replacements):
    valid_replacements = {}
    # Fase 1: old → placeholder
    for old, new, placeholder in replacements:
        if old in text:
            text = text.replace(old, placeholder)
            valid_replacements[placeholder] = new
    # Fase 2: placeholder → new
    for placeholder, new in valid_replacements.items():
        text = text.replace(placeholder, new)
    return text
```

### 8.2 Gestione delle Annotazioni Ruby

Il codice per la gestione delle annotazioni Ruby HTML è particolarmente interessante:

```python
def output_format(main_text, ruby_content, format_type, char_widths_dict):
    if format_type == 'HTML格式_Ruby文字_大小调整':
        # Calcolo della larghezza per adattare dimensione ruby
        width_ruby = measure_text_width_Arial16(ruby_content, char_widths_dict)
        width_main = measure_text_width_Arial16(main_text, char_widths_dict)
        ratio_1 = width_ruby / width_main
        
        # Scelta della classe CSS in base al rapporto
        if ratio_1 > 6:
            return f'<ruby>{main_text}<rt class="XXXS_S">{insert_br_at_third_width(ruby_content, char_widths_dict)}</rt></ruby>'
        elif ratio_1 > (9/3):
            return f'<ruby>{main_text}<rt class="XXS_S">{insert_br_at_half_width(ruby_content, char_widths_dict)}</rt></ruby>'
        # ... altre classi in base al rapporto
```

Questo codice calcola il rapporto tra la larghezza del testo ruby e del testo principale per selezionare la classe CSS appropriata. Inserisce anche tag `<br>` nei testi ruby più lunghi per migliorare la leggibilità.

### 8.3 Elaborazione Parallela dei Segmenti

L'implementazione dell'elaborazione parallela dei segmenti di testo:

```python
def process_segment(lines, placeholders_for_skipping_replacements, replacements_list_for_localized_string,
                    placeholders_for_localized_replacement, replacements_final_list,
                    replacements_list_for_2char, format_type):
    # Unione delle linee in un segmento
    segment = ''.join(lines)
    
    # Elaborazione del segmento
    result = orchestrate_comprehensive_esperanto_text_replacement(
        segment,
        placeholders_for_skipping_replacements,
        replacements_list_for_localized_string,
        placeholders_for_localized_replacement,
        replacements_final_list,
        replacements_list_for_2char,
        format_type
    )
    
    return result
```

Questa funzione viene chiamata in parallelo da diversi processi, ognuno dei quali elabora un segmento di testo diverso.

### 8.4 Algoritmo Principale di Sostituzione

La funzione principale di orchestrazione delle sostituzioni:

```python
def orchestrate_comprehensive_esperanto_text_replacement(text, placeholders_for_skipping_replacements,
                                                      replacements_list_for_localized_string,
                                                      placeholders_for_localized_replacement,
                                                      replacements_final_list, replacements_list_for_2char,
                                                      format_type):
    # 1. Normalizzazione e conversione caratteri esperanto
    text = unify_halfwidth_spaces(text)
    text = convert_to_circumflex(text)
    
    # 2. Gestione parti da preservare (%...%)
    replacements_list_for_intact_parts = create_replacements_list_for_intact_parts(...)
    for original, place_holder_ in sorted_replacements_list_for_intact_parts:
        text = text.replace(original, place_holder_)
    
    # 3. Gestione sostituzioni localizzate (@...@)
    tmp_replacements_list_for_localized_string_2 = create_replacements_list_for_localized_replacement(...)
    for original, place_holder_, replaced_original in sorted_replacements_list_for_localized_string:
        text = text.replace(original, place_holder_)
    
    # 4. Sostituzione globale
    valid_replacements = {}
    for old, new, placeholder in replacements_final_list:
        if old in text:
            text = text.replace(old, placeholder)
            valid_replacements[placeholder] = new
    
    # 5. Sostituzione radici 2 caratteri (2 passaggi)
    # ...
    
    # 6. Ripristino placeholder
    # ...
    
    # 7. Formattazione HTML se necessario
    if "HTML" in format_type:
        text = text.replace("\n", "<br>\n")
        # ...
    
    return text
```

Questo algoritmo implementa l'intera sequenza di elaborazione del testo.

---

## Conclusione

Questa applicazione Streamlit per la sostituzione di testo in esperanto è un esempio sofisticato di elaborazione del testo con particolare attenzione all'ottimizzazione delle prestazioni, alla gestione di casi particolari e alla presentazione visiva dei risultati.

Le tecniche chiave utilizzate includono:
- Sostituzione sicura attraverso l'uso di placeholder
- Ottimizzazione delle priorità di sostituzione basate sulla lunghezza del testo
- Elaborazione parallela per migliorare le prestazioni
- Caching in Streamlit per evitare operazioni costose
- Gestione avanzata delle annotazioni HTML Ruby con dimensionamento automatico

Queste tecniche possono essere applicate anche in altri progetti di elaborazione del testo o applicazioni Streamlit che richiedono prestazioni elevate e una presentazione visiva sofisticata.