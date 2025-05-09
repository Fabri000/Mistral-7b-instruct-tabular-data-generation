# Mistral-7b Instruct Tabular data generation
## Dipendenze
Principali librerie necessarie:
* selenium 4.24.0
* jsonlines
* pandas 2.2.2
* torch 2.4.0
* bitsandbytes 0.44.1
* huggingface 0.25.1
* transformers 4.45.1
* peft
* numpy 1.26.4

## Project overview
Effettuare il finetuning del modello Mistral-7b Instruct per generare dati in forma tabellare consistenti in piani di allenamento per la palestra. 
Alla base del progetto vi è una fase preliminare di webscraping per la creazione di un dataset nella forma di insieme di coppie domanda/risposta costituito da una richiesta ed un piano di allenamento.

## Dataset estratto per effettuare il finetuning
Il dataset su cui è stato effettuato il finetuning del modello è stato estratto usando selenium per realizzare un web scraper per il sito web jefit.com. 
I dati contenuti nel sito web sono stati rielaborati in modo tale da realizzare un dataset composto da coppie di domande e risposte, dove le domande erano costituite dalle parole chiave associate al piano e le risposte opportunamente formattate in formato json. \
Il dataset finale è costituito 5896 coppie domanda/risposta le quali sono poi suddivise tra insieme usato per valutare il modello e per addestrarlo.

## Metrica per la valutazione del modello
Per via del fatto che si ha a disposizione la risposta originale alla domanda che si usa come input per il modello, una metrica valida per effettuare la valutazioni delle performance è stata lo score BLEU, che consiste nel confrontare il matching tra l'output generato dal modello sull'imput proposto e la ground truth.

## Esperimenti e risultati
Il primo esperimento è consistito nell'effettuare il finetuning del modello sfruttando il dataset costruito e confrontare il risultato rispetto al modello preaddestrato.
In particolare, si è considerata la media dello score Bleu su un insieme di esempi sul quale il modello non è stato addestrato pari all'1% del dataset originale.
|**Modello**| **Mean Bleu** |
|-----------| :-----------: |
|**Pretrained**| 0 |
|**Finetuned**| 0.3118 |
Per migliorare le capacità del modello rispetto lo score BLEU si è provato ad sfruttare, in fase di inferenza, versioni di in-context learning
|**Prompting**| **Mean Bleu** |
|-----------| :-----------: |
|**Zero shot**| 0.3118 |
|**One shot**| 0.4126 |
|**Two shot**| 0.3578 |
e da ciò si osserva come l'inserimento di un esempio nel prompt, in inferenza, permetta di migliorare le performance del modello, sebbene aumentando il numero di esempio oltre il singolo esempio fa degradare le performance generali dello stesso.\
Successivamente si è provato ad aumentare la performance del modello finetuned provando ad usare tecniche di prompting più avanzate come il **role play prompting** e il **self augmented promtping** 
|**Tecnica di prompting**| **Mean Bleu** |
|-----------| :-----------: |
|**Role-Play**| 0.2943 |
|**Self Augmented**| 0.2528 |
ma non si sono osservati miglioramenti significativi nelle performance.\
Per questo motivo si è provato, infine, a combinare le versioni di in-context e tecniche avanzate di prompting più promettenti per valutare le performance del modello
| Combination              | Mean BLEU |
| ------------------------ | :---------: |
| **1-shot & role-play**      | 0.3609|
| **self-augment & role-play** | 0.1267|
| **1-shot & self-augment**    | 0.4740|

