# EsameGennaio2020
Repository dedicata all'esame di Programmazione ad Oggetti del 20 gennaio 2020.
È stata effettuata una revisione del codice precedentemente caricato (Dicembre 2019). In particolare:
È stata migliorata l'organizzazione nei package; le classi AppService e Filters sono state modificate tramite l'aggiunta di nuovi metodi e una revisione dei precedenti; i vecchi diagrammi UML sono stati sostituiti; sono state inoltre aggiunte nuove funzionalità per le rotte di tipo POST e DELETE.

Il progetto in questione fornisce un'applicazione WEB in JAVA, implementata attraverso l'utilizzo di SpringBoot, che prevede la modellazione di un dataset in formato TSV reperibile al seguente URL: http://data.europa.eu/euodp/data/api/3/action/package_show?id=3h1d8YdPl3KCgkeQNbjkA

# Inizializzazione Dataset
All’avvio del programma viene eseguita l’elaborazione del dataset attraverso la classe _Utils_, che decodifica il JSON dell'URL sopra indicato per poi effettuarne il download e successivamente il parsing.
Il risultato è l’organizzazione dei dati in una tabella le cui righe rappresentano gli elementi del dataset in questione.
Ogni riga (elemento) è contraddistinta da un indice che ne precisa la rispettiva posizione all'interno della struttura dati e contiene dei valori riguardanti informazioni statistiche sulla popolazione; tali informazioni sono distribuite in 38 colonne, di cui:

le prime cinque colonne, identificate rispettivamente dagli indici duration, deg_urb, sex, age, unit, contengono elementi di tipo stringa, mentre le successive contengono elementi numerici (i valori di timegeo e degli indici dei paesi europei sono rispettivamente di tipo intero e float).

# Avvio
L’applicazione avvia un web-server in locale sulla porta 8080 che rimane in attesa di richieste.
Selezionando la rotta desiderata (GET, POST o DELETE) è possibile inserire delle richieste che prevedono la restituzione di particolari dati a seconda del comando desiderato.
URL per l’applicazione:
> _/http//localhost:8080_

# GET
Selezionando la rotta GET si possono immettere i seguenti comandi:

**Restituzione Dati**
•	` _/data_` -> restituisce tutti gli elementi del dataset, ognuno con le rispettive specifiche.  
•	` _/data/{index}_` -> restituisce le specifiche dell’i-esimo elemento.  
• `	_/metadata_` -> restituisce i metadati, evidenziando il nome dei rispettivi campi (field) e il tipo di valore contenuto al suo interno.

**Restituzione Statistiche**
Per ogni campo contenente un valore di tipo _float_ è possibile eseguire delle operazioni matematiche tramite le seguenti richieste:
•	`_/sum/{field}_` -> restituisce la somma di tutti i valori contraddistinti dal campo _field_
• `_/min/{field}_` -> restituisce il minimo tra tutti i valori contraddistinti dal campo _field_
• `_/max/{field}_` -> restituisce il massimo tra tutti i valori contraddistinti dal campo _field_
• `_/avg/{field}_` -> restituisce la media aritmetica dei valori contraddistinti dal campo _field_
• `_/devstd/{field}_` -> restituisce la deviazione standard dei valori contraddistinti dal campo _field_

• `_/stats/{field}_`-> restituisce l'elenco delle statistiche sopra riportate per il _field_ scelto

Esempio:
>_/min/EU28_ = 1.3 -> restituisce il più piccolo dei valori contenuti nella colonna EU28
> _/average/IT_ = 44.705803 -> restituisce la media dei valori presenti nella colonna IT



**Filtri semplici:**

Al contrario delle statistiche, le operazioni di filtraggio possono essere eseguite per tutti i campi. In particolare è possibile effettuare le seguenti richieste:
- Per attributi di tipo stringa -> `_/stringFilter/{field}/{word}_`
Restituisce tutti gli elementi che contengono nel rispettivo campo _field_ la stringa _word_

Esempio:
> _/stringFilter/EU28/NEV_' -> restituisce tutti gli elementi contenenti la stringa NEV nella colonna EU28.  

- Per gli attributi numerici -> `_/valueFilter/{field}/{operator}/{value}_`
_operator_ rappresenta l’operatore di confronto; può essere scelto **>**, **<**, **=**.
_value_ rappresenta il valore di soglia da confrontare.
Restituisce tutti gli elementi che rispettano la condizione imposta dal confronto.

Esempio:
> _/valueFilter/EU28/>/50_ -> restituisce tutti gli elementi i cui valori nella colonna EU28 sono maggiori di 50.  
> _/valueFilter/timegeo/=/2014_ -> restituisce tutti gli elementi i cui valori nella colonna timegeo sono uguali 2014.

**Filtri combinati:**

-	Doppio filtro per attributi numerici -> `_/valueFilter/{field}/{operator1}/{value1}/{logicOperator}/{operator2}/{value2}_`
Rappresenta una combinazione di due filtri numerici mostrati sopra.
_logicOperator_ indica l'operatore logico; può essere scelto **and** o **or** a seconda che si vogliano rispettare entrambe le condizioni o solamente una delle due (intersezione o unione degli elementi filtrati).
_operator1_ e _operator2_ sono gli operatori di confronto precedentemente indicati.
_value1_ _value2_ sono i valori da confrontare.
Restituisce l'elenco degli elementi che soddisfano i requisiti imposti.

Esempio:
> _/valueFilter/EU28/>/50/or/</20_ -> restituisce gli elementi i cui valori nella colonna EU28 sono maggiori di 50 o minori di 20
> _/valueFilter/EU28/>/50/and/</20_ -> restituisce gli elementi i cui valori nella colonna EU28 sono contemporaneamente maggiori di 50 e minori di 20

-	Filtro combinato: attributi  numerici e stringhe -> `_/filter/{field1}/{word}/{field2}/{operator}/{value}_`
Rappresenta una combinazione di un filtro numerico e un filtro stringa.
_field1_ e _field2_ sono i campi contenenti rispettivamente attributi di tipo stringa e attributi numerici.
_word_ rappresenta la stringa da confrontare mentre _value_ rappresenta il valore di soglia.
Restituisce l'elenco di elementi che contengono la stringa _word_ nella colonna _field1_ e allo stesso tempo rispettano la condizione imposta dal confronto numerico.

Esempio:
> _/filter/duration/NEV/EU28/</50_ -> restituisce tutti gli elementi che contengono NEV nella colonna duration e allo stesso tempo contengono un valore minore di 50 nella colonna EU28

# POST
Selezionando la rotta POST è possibile inserire nuovi dati in formato JSON tramite la richiesta: '/data'
Una volta inviata, occorre spostarsi nel campo **Body** selezionare **raw** e scegliere il formato JSON nel menu **Text**; fatto ciò basterà scrivere i nuovi dati racchiusi da parentesi graffe:

# DELETE

# Diagrammi:
