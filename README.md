# Mistral-7b Instruct Tabular data generation
## Dipendenze
Principali librerie necessarie:
* selenium 4.24.0
* jsonlines
* pandas
* torch 2.4.0
* bitsandbytes 0.44.1
* huggingface 0.25.1
* transformers 4.45.1
* peft

## Project overview
Effettuare il finetuning del modello Mistral-7b Instruct per generare dati in forma tabellare consistenti in piani di allenamento per la palestra. 
Alla base del progetto vi è una fase preliminare di webscraping per la creazione di un dataset nella forma di insieme di coppie domanda/risposta costituito da una richiesta ed un piano di allenamento.

## Dataset estratto per effettuare il finetuning
Il dataset su cui è stato effettuato il finetuning del modello è stato estratto usando selenium per realizzare un web scraper per il sito web jefit.com. 
I dati contenuti nel sito web sono stati rielaborati in modo tale da realizzare un dataset composto da coppie di domande e risposte, dove le domande erano costituite dalle parole chiave associate al piano e le risposte opportunamente formattate in formato json.
La tokenizzazione è stata realizzata usando il tokenizer Mistral.

## Metrica per la valutazione del modello
Per via del fatto che si ha a disposizione la risposta originale alla domanda che si usa come input per il modello, una metrica valida per effettuare la valutazioni delle performance è stata lo score BLEU.
Esso consiste nel confrontare il matching tra l'output generato dal modello sull'imput proposto e la ground truth.

## Esperimenti e risultati
Il primo esperimento è consistito nell'effettuare il finetuning del modello sfruttando il dataset costruito e confrontare il risultato rispetto al modello preaddestrato.
|**Modello**| **Mean Bleu** |
|-----------| :-----------: |
|**Pretrained**| 0 |
|**Finetuned**| 0.3118 |
