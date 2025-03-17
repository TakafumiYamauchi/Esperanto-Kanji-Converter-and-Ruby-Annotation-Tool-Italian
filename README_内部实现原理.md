# Guida Tecnica: Strumento di sostituzione di caratteri (kanji) per il testo in esperanto

## Indice
1. Architettura Generale dell'Applicazione
2. Componenti Principali e Flusso di Dati
3. Strutture Dati e File
4. Algoritmi Chiave
5. Elaborazione Parallela
6. Dettagli Implementativi
7. Tecniche Avanzate

## 1. Architettura Generale dell'Applicazione

### 1.1 Panoramica

L'applicazione è costruita su Streamlit e consiste in quattro componenti principali:

1. **main.py**: Il punto di ingresso dell'applicazione che gestisce l'interfaccia utente principale e la funzionalità di sostituzione del testo.
2. **Pagina per generare file JSON...**: Una pagina secondaria che permette agli utenti di generare file JSON personalizzati contenenti le regole di sostituzione.
3. **esp_text_replacement_module.py**: Un modulo di supporto che contiene funzioni per la sostituzione e l'elaborazione del testo.
4. **esp_replacement_json_make_module.py**: Un modulo che fornisce funzioni per creare regole di sostituzione in formato JSON.

L'architettura segue un modello di elaborazione a pipeline, dove il testo in esperanto passa attraverso diverse fasi di trasformazione prima di essere visualizzato all'utente.

### 1.2 Flusso di Controllo

Il flusso di controllo principale dell'applicazione è il seguente:

1. L'utente carica o inserisce un testo in esperanto
2. L'applicazione carica regole di sostituzione da un file JSON
3. Il testo viene elaborato attraverso una serie di trasformazioni
4. Il risultato viene visualizzato e può essere scaricato

Nella pagina secondaria, il flusso è invece:

1. L'utente carica un file CSV con corrispondenze tra radici esperanto e traduzioni
2. L'utente carica o utilizza JSON predefiniti per le regole di scomposizione
3. L'applicazione genera un nuovo file JSON combinato che può essere scaricato e utilizzato nella pagina principale

## 2. Componenti Principali e Flusso di Dati

### 2.1 Main.py

#### 2.1.1 Inizializzazione e Configurazione

```python
# Configurazione della pagina Streamlit
st.set_page_config(
    page_title="Strumento di sostituzione di caratteri (kanji) per il testo in esperanto",
    layout="wide"
)
```

La funzione `set_page_config` imposta il titolo della pagina e il layout ampio per un'interfaccia più spaziosa.

#### 2.1.2 Caricamento dei File JSON

```python
@st.cache_data
def load_replacements_lists(json_path: str) -> Tuple[List, List, List]:
    # Carica il file JSON e restituisce tre liste di sostituzioni
```

Questa funzione decorata con `@st.cache_data` carica le liste di sostituzione dal file JSON e memorizza i risultati nella cache di Streamlit. Utilizza la memorizzazione nella cache per evitare di ricaricare file JSON di grandi dimensioni (fino a 50MB) a ogni interazione dell'utente, migliorando significativamente le prestazioni.

#### 2.1.3 Elaborazione del Testo

Il cuore della funzionalità di sostituzione si trova nel blocco:

```python
if submit_btn:
    st.session_state["text0_value"] = text0
    if use_parallel:
        processed_text = parallel_process(...)
    else:
        processed_text = orchestrate_comprehensive_esperanto_text_replacement(...)
```

Qui l'applicazione decide se utilizzare l'elaborazione parallela in base all'input dell'utente e chiama la funzione appropriata.

### 2.2 Pagina Secondaria (Generazione JSON)

La pagina secondaria è ottimizzata per generare file JSON personalizzati. Utilizza un approccio a fasi:

1. Carica un CSV con corrispondenze di base
2. Elabora le regole di scomposizione
3. Crea vari tipi di sostituzioni (globali, localizzate, per radici di 2 caratteri)
4. Combina tutto in un unico file JSON

Il cuore della generazione avviene quando l'utente clicca sul pulsante "Crea il file JSON per la sostituzione":

```python
if st.button("Crea il file JSON per la sostituzione"):
    with st.spinner("Generazione del file JSON..."):
        # Elaborazione in 16 passi
        # ...
        combined_data = {
            "全域替换用のリスト(列表)型配列(replacements_final_list)": replacements_final_list,
            "二文字词根替换用のリスト(列表)型配列(replacements_list_for_2char)": replacements_list_for_2char,
            "局部文字替换用のリスト(列表)型配列(replacements_list_for_localized_string)": replacements_list_for_localized_string
        }
```

### 2.3 Modulo esp_text_replacement_module.py

Questo modulo contiene le funzioni fondamentali per la sostituzione del testo:

```python
def orchestrate_comprehensive_esperanto_text_replacement(
    text,
    placeholders_for_skipping_replacements,
    replacements_list_for_localized_string,
    placeholders_for_localized_replacement,
    replacements_final_list,
    replacements_list_for_2char,
    format_type
) -> str:
    # Implementa l'algoritmo principale di sostituzione
```

Questa funzione principale orchestra tutto il processo di sostituzione in un'unica chiamata di funzione.

### 2.4 Modulo esp_replacement_json_make_module.py

Questo modulo fornisce funzioni di supporto per la creazione di file JSON:

```python
def output_format(main_text, ruby_content, format_type, char_widths_dict):
    # Formatta il testo in base al formato di output selezionato
```

Questa funzione è cruciale per determinare come il testo sostituito verrà visualizzato nell'output.

## 3. Strutture Dati e File

### 3.1 File JSON

Il file JSON principale contiene tre liste chiave:

1. **`replacements_final_list`**: Lista per sostituzioni globali (全域替换用)
2. **`replacements_list_for_2char`**: Lista per sostituzioni di radici a 2 caratteri (二文字词根替换用)
3. **`replacements_list_for_localized_string`**: Lista per sostituzioni localizzate (局部文字替换用)

Ogni elemento in queste liste ha la struttura `[old, new, placeholder]`, dove:
- `old`: testo originale da sostituire
- `new`: testo sostituito
- `placeholder`: stringa temporanea usata durante l'elaborazione

### 3.2 File CSV e Struttura Dati

I file CSV contengono mappature tra radici esperanto e traduzioni:

```
esperanto_root,translation
hom,uomo
parol,parlare
...
```

Internamente, queste mappature vengono trasformate in strutture dati più complesse:

```python
# Esempio di una voce in replacements_final_list
["parol", "<ruby>parol<rt class=\"M_M\">parlare</rt></ruby>", "$20987$"]
```

### 3.3 Placeholders

I placeholder sono stringhe temporanee usate durante il processo di sostituzione per evitare sostituzioni sovrapposte:

```python
placeholders_for_skipping_replacements = import_placeholders(
    './Appの运行に使用する各类文件/占位符(placeholders)_%1854%-%4934%_文字列替换skip用.txt'
)
```

Questi sono caricati da file di testo e utilizzati nelle fasi di sostituzione per garantire che le parti marcate con `%...%` non vengano sostituite.

## 4. Algoritmi Chiave

### 4.1 Algoritmo di Sostituzione Principale

L'algoritmo di sostituzione principale è implementato nella funzione `orchestrate_comprehensive_esperanto_text_replacement()`:

```python
def orchestrate_comprehensive_esperanto_text_replacement(text, ...):
    # 1, 2) Normalizzazione degli spazi + Conversione in forma con circonflesso
    text = unify_halfwidth_spaces(text)
    text = convert_to_circumflex(text)

    # 3) Sostituzione temporanea delle parti %...% da saltare
    replacements_list_for_intact_parts = create_replacements_list_for_intact_parts(...)

    # 4) Sostituzione localizzata @...@
    tmp_replacements_list_for_localized_string_2 = create_replacements_list_for_localized_replacement(...)

    # 5) Sostituzione globale
    valid_replacements = {}
    for old, new, placeholder in replacements_final_list:
        if old in text:
            text = text.replace(old, placeholder)
            valid_replacements[placeholder] = new

    # 6) Sostituzione delle radici di 2 caratteri (2 volte)
    # ...

    # 7) Ripristino dei placeholder al testo finale
    # ...

    # 8) Formato HTML se richiesto
    # ...

    return text
```

Questo algoritmo multi-fase garantisce che:
1. Le parti marcate con `%...%` non vengano sostituite
2. Le parti marcate con `@...@` abbiano sostituzioni localizzate
3. Le sostituzioni più lunghe avvengano prima delle più corte
4. Le radici di 2 caratteri ricevano un trattamento speciale

### 4.2 Algoritmo di Sostituzione Sicura

La funzione `safe_replace()` è fondamentale per il processo di sostituzione:

```python
def safe_replace(text: str, replacements: List[Tuple[str, str, str]]) -> str:
    valid_replacements = {}
    # Prima old→placeholder
    for old, new, placeholder in replacements:
        if old in text:
            text = text.replace(old, placeholder)
            valid_replacements[placeholder] = new
    # Poi placeholder→new
    for placeholder, new in valid_replacements.items():
        text = text.replace(placeholder, new)
    return text
```

Questa funzione risolve elegantemente il problema delle sostituzioni sovrapposte utilizzando i placeholder come passaggio intermedio.

### 4.3 Algoritmo di Generazione JSON

Nella pagina secondaria, l'algoritmo per generare file JSON personalizzati comporta 16 passaggi principali, tra cui:

1. Caricamento e normalizzazione dei dati CSV
2. Creazione di un dizionario temporaneo di sostituzioni
3. Ordinamento per lunghezza (priorità alle sostituzioni più lunghe)
4. Generazione di placeholder per evitare collisioni
5. Applicazione di regole di scomposizione personalizzate
6. Trattamento speciale per suffissi verbali, "AN", "ON", ecc.
7. Generazione di varianti con maiuscole e minuscole
8. Organizzazione in tre liste finali

## 5. Elaborazione Parallela

### 5.1 Implementazione del Multiprocessing

L'applicazione utilizza il modulo `multiprocessing` di Python per migliorare le prestazioni con testi lunghi:

```python
def parallel_process(
    text: str,
    num_processes: int,
    # altri parametri...
) -> str:
    # Suddivide il testo in blocchi
    lines = re.findall(r'.*?\n|.+$', text)
    # Calcola intervalli per ogni processo
    # Esegue process_segment in parallelo
    with multiprocessing.Pool(processes=num_processes) as pool:
        results = pool.starmap(process_segment, [...])
    return ''.join(results)
```

Questa funzione divide il testo in blocchi e li elabora in parallelo, per poi riunire i risultati. All'inizio di `main.py`, viene impostato il metodo `spawn` per evitare errori di serializzazione:

```python
try:
    multiprocessing.set_start_method("spawn")
except RuntimeError:
    pass  # Ignora se il metodo è già impostato
```

### 5.2 Strategia di Parallelizzazione

La strategia utilizzata è quella di dividere il testo per righe e assegnare gruppi di righe a processi separati. Questo approccio funziona bene perché:

1. La maggior parte delle sostituzioni avviene all'interno di righe singole
2. C'è poca interdipendenza tra righe diverse
3. I dati di sostituzione vengono condivisi tra tutti i processi

Nella generazione di JSON, viene utilizzato un approccio simile per la costruzione del dizionario di pre-sostituzione:

```python
def parallel_build_pre_replacements_dict(
    E_stem_with_Part_Of_Speech_list: List[List[str]],
    replacements: List[Tuple[str, str, str]],
    num_processes: int = 4
) -> Dict[str, List[str]]:
    # Divide i dati in chunk
    # Elabora ogni chunk in parallelo
    # Unisce i risultati
```

## 6. Dettagli Implementativi

### 6.1 Gestione dei Caratteri Speciali dell'Esperanto

L'applicazione supporta diverse rappresentazioni dei caratteri speciali dell'esperanto:

1. **Circonflesso** (ĉ, ĝ, ĥ, ĵ, ŝ, ŭ)
2. **Formato x** (cx, gx, hx, jx, sx, ux)
3. **Formato ^** (c^, g^, h^, j^, s^, u^)

```python
x_to_circumflex = {
    'cx': 'ĉ', 'gx': 'ĝ', 'hx': 'ĥ', 'jx': 'ĵ', 'sx': 'ŝ', 'ux': 'ŭ',
    'Cx': 'Ĉ', 'Gx': 'Ĝ', 'Hx': 'Ĥ', 'Jx': 'Ĵ', 'Sx': 'Ŝ', 'Ux': 'Ŭ'
}

def convert_to_circumflex(text: str) -> str:
    text = replace_esperanto_chars(text, hat_to_circumflex)
    text = replace_esperanto_chars(text, x_to_circumflex)
    return text
```

### 6.2 Formattazione HTML e Annotazioni Ruby

L'applicazione gestisce diverse opzioni di formattazione, con particolare attenzione al tag HTML `<ruby>`:

```python
def output_format(main_text, ruby_content, format_type, char_widths_dict):
    if format_type == 'HTML格式_Ruby文字_大小调整':
        # Calcola le proporzioni e formatta di conseguenza
        width_ruby = measure_text_width_Arial16(ruby_content, char_widths_dict)
        width_main = measure_text_width_Arial16(main_text, char_widths_dict)
        ratio_1 = width_ruby / width_main

        if ratio_1 > 6:
            return f'<ruby>{main_text}<rt class="XXXS_S">{insert_br_at_third_width(ruby_content, char_widths_dict)}</rt></ruby>'
        # ...altre condizioni
    # ...altri formati
```

Questa funzione utilizza proporzioni tra la lunghezza del testo originale e la traduzione per determinare la classe CSS da applicare, che controlla la dimensione del testo ruby.

### 6.3 Miglioramenti Estetici

L'applicazione include diverse ottimizzazioni estetiche:

```python
def remove_redundant_ruby_if_identical(text: str) -> str:
    """
    Rimuove <ruby> quando il testo genitore e ruby sono identici
    """
```

```python
def capitalize_ruby_and_rt(text: str) -> str:
    """
    Applica la maiuscola al primo carattere sia del testo genitore che del ruby
    """
```

Queste funzioni migliorano la presentazione visiva del risultato finale, rimuovendo le annotazioni ridondanti e applicando correttamente le maiuscole.

## 7. Tecniche Avanzate

### 7.1 Memorizzazione nella Cache di Streamlit

L'applicazione utilizza il decoratore `@st.cache_data` per ottimizzare le operazioni costose:

```python
@st.cache_data
def load_replacements_lists(json_path: str) -> Tuple[List, List, List]:
    # Carica il file JSON e restituisce tre liste
```

Questo garantisce che il file JSON venga letto dal disco una sola volta, anche se l'utente interagisce ripetutamente con l'applicazione.

### 7.2 Espressioni Regolari

L'applicazione utilizza espressioni regolari per varie operazioni di elaborazione del testo:

```python
# Pattern per trovare testo tra %...%
PERCENT_PATTERN = re.compile(r'%(.{1,50}?)%')

# Pattern per trovare testo tra @...@
AT_PATTERN = re.compile(r'@(.{1,18}?)@')

# Pattern per trovare tag ruby con testo identico
IDENTICAL_RUBY_PATTERN = re.compile(r'<ruby>([^<]+)<rt class="XXL_L">([^<]+)</rt></ruby>')
```

### 7.3 Trattamento Speciale per Casi Linguistici

L'applicazione include logiche speciali per casi particolari della lingua esperanto:

```python
# Suffissi verbali
verb_suffix_2l = {
    'as':'as', 'is':'is', 'os':'os', 'us':'us','at':'at','it':'it','ot':'ot',
    'ad':'ad','iĝ':'iĝ','ig':'ig','ant':'ant','int':'int','ont':'ont'
}

# Trattamento "AN" (suffisso per membri)
AN=[['dietan', '/diet/an/', '/diet/an'], ...]

# Trattamento "ON" (suffisso per frazioni)
ON=[['duon', '/du/on/', '/du/on'], ...]
```

Questi casi speciali ricevono trattamenti personalizzati durante la generazione del file JSON, permettendo sostituzioni più precise e linguisticamente accurate.

## Conclusione

L'applicazione "Strumento di sostituzione di caratteri (kanji) per il testo in esperanto" è un esempio sofisticato di elaborazione linguistica con Streamlit. La sua architettura modulare, l'uso efficace di strutture dati, l'elaborazione parallela e l'attenzione ai dettagli linguistici ne fanno uno strumento potente e flessibile.

Come programmatore, puoi estendere questa applicazione in vari modi:
1. Aggiungendo supporto per altre lingue di destinazione
2. Migliorando gli algoritmi di sostituzione per casi speciali
3. Ottimizzando ulteriormente le prestazioni
4. Aggiungendo funzionalità di analisi linguistica

Spero che questa guida tecnica ti abbia fornito una comprensione approfondita del funzionamento interno dell'applicazione e ti ispiri a esplorare e migliorare ulteriormente questo interessante progetto.
