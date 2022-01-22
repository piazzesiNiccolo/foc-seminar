# Intro

Buongiorno a tutti, nel seminario di oggi andrò a parlarvi di rewriting logic e del linguaggio Maude. Partiremo da una breve introduzione volta a contestualizzare e motivare lo sviluppo di tale modello formale, dopodichè andremo ad esplorare la teoria sia dal punto di vista sintattico, andando a vedere come sono formati i termini della rewriting logic e le regole di inferenza, sia dal punto di vista semantico, andando invece ad approfondire il modello formale 2-categoriale che fornisce una precisa definizione matematica del comportamento di tale logica. Successivamente , faremo un breve excursus sui concetti fondamentali di Maude, linguaggio dichiarativo basato proprio sulla rewriting logic. Attraverso brevi esempi vedremo i concetti fondamentali e gli strumenti messi a disposizione dal linguaggio  per la prototipizzazione di modelli computazionali e logiche. Nell'ultima vedremo, vedremo alcune efficaci applicazioni di Maude, approfondendo in particolare un case study che riguarda la formalizzazione e l'estensione di Megastore, infrastruttura utilizzata da google per garantire la consistenza di transazioni in ambiente distribuito.

# Why rewriting logic

Partiamo da una delle domande più studiate dell'informatica: cos'è e come puo essere precisamente rappresentato un sistema concorrente? Nel corso dei decenni varie risposte sono state proposte, ciascuna delle quali focalizzata su una particolare intepretazione di concorrenza. Alcune delle più "famose"  qui riportate come ad esempio, CCS CSP, Actor model ma anche modelli piu generali come la chemical abstract machine, il $\pi$-calcolo o modelli basati su logiche modali .   Tutti questi modelli hanno avuto il loro successo, nel risolvere determinati problemi ma nel corso degli anni è emerso il problema della *frammentazione*.  Vi è un problema di frammentazione esterna, in quanto risulta complicato mettere in relazione approcci divers, basati su concetti diversi per risolvere problemi diversi, ma esiste anche un problema di frammentazione interna. Difatti all'interno dello stesso approccio possono essere utilizzati diversi  modelli per descrivere i comportamenti di un sistema. Ad esempio, come è riportato nelle slide, risulta complesso unificare semantica operazionale e denotazionale di modelli computazionali complessi come il CCS. Un ulteriore problema è quello  di introdurre computazioni concorrenti in altri paradigmi (funzionale, OOP). Solitamente il risultato finale sono soluzioni ad hoc che forzano in modo non nativo la concorrenza, risultando in modelli molto piu complessi e difficili da analizzare.  

L'obiettivo della rewriting logic è proprio quello di *unificare* approcci non collegati in un unico modello astratto capace di descriverli come specifiche istanza del modello stesso. L'approccio  utilizzato  è a due lati:

- Dal punto di vista computazionale/algoritmico, la rewriting logic è un framework semnatico capace drappresentare ed eseguire molti modelli e linguaggi come **teorie di riscrittura**, che è il concetto fondamentale della logica.

- la RL è un framework anche logico, perchè attraverso di essa è possibile modellare ragionare, e automatizzare altre logiche e sistemi di prova.

  L'idea chiave è quella riportata : nella RL

# Syn



# Sem

# Maude 

# Google

