# Intro 



Buongiorno a tutti, oggi andrò a parlarvi di Rewriting Logic e Maude. 

Partendo da una breve introduzione

 la domanda da cui partiamo è: Cos'è un sistema concorrente? Questo è forse uno dei problemi più studiati dell'informatica e 

nel corso dei decenni sono state proposte varie soluzioni come ad esempio ccs, csp, reti di petri o modelli più generali come ad esempio chemical abstract machine  o pi calcolo o modelli basati su logiche temporali e modali.

Il problema dell'avere così tanti approcci  è quello della *frammentazione*. È infatti complesso cercare di relazionare modelli molto diversi fra loro  che utilizzano diverse nozioni di base per risolvere diversi problemi.

Vi è anche  quella che qui è chiamata  frammentazione interna. Infatti anche all'interno dello stesso modello dove ad esempio risulta complesso unificare semantica operazionale e denotazionale in modo soddisfacente.

Un altra questione è quella dell'integrazione della concorrenza in altre aree, come ad esempio nei linguaggi di programmazione dove spesso si usano soluzioni *ad hoc* non native che risultano in modelli molto più complessi del dovuto. L'obiettivo della rewriting logic è quello di unificare semanticamente tutti i paradigmi attraverso il concetto di teoria di riscrittura e di fornire una visione universale dei sistemi di concorrenza e di strumenti per ragionare in modo rigoroso e corretto su di essi. 

La strategia seguita è quella riportata nel diagramma: Prima di tutto si descrive il modello astratto, che fornisce la rigorosa descrizione matematica dei modelli e la loro semantica. Dopodichè si utilizza il modello come base di sviluppo per linguaggi di alto livello, che in questo caso si traduce in *Maude*, del quale parleremo successivamente. Viene poi sviluppato un ambiente di programmazione dotato di metaprogrammazione (questo grazie alle caratteristiche riflessive della logica stessa) e questo ambiente viene infine utilizzato per descrivere  e ragionare su sistemi concreti e realistici. 





# Foundations

Partiamo dalla  descrizione del modello. 

In un  primo esempio molto semplice vediamo come un LTS viene rappresentato come  teoria di riscrittura attraverso un modulo Maude. Come possiamo vedere abbiamo una parte algebrica, dove descriviamo i sort presenti e i simboli di operazioni che in questo caso sono semplicmeenti stati e i loro nomi. La novità sono queste "regole" che rappresentano le transizioni del sistema. Un LTS può essere visto come una teoria di riscrittura semplificata, infatti non abbiamo variabili ma soltanto costanti e non abbiamo concorrenza ma soltanto non-determinismo, che invece pososno essere presenti nel caso generale. 

Adesso andiamo a vedere la descrizione formale di una teoria di rsicrittura. Una teoria di riscrittura  è una quadrupla $\Sigma$,E ,L,R dove  Sigma è  un insieme ordinato di simboli di funzioni  e E un insieme di Sigma-equazioni L è un insieme di etichette e R è un insieme di coppie  dove il primo componente è un'etichetta e il secondo è una sequenza non vuota di classi di equivalenza di (Sigma,E) termini  parametrizzati da un insieme enumerabile di variabili.

Elementi in R vengono chiamati regole di riscrittura che possiamo leggere come

t diventa t' se u1 diventa u1' u2 diventa u2

Le regole di riscrittura operano su classi di equivalenza di termini costruiti su sigma modulo  le equazioni in E . Questopermette operare astraendo dalla specifica sintassi e fornendo una maggior flessibilità. Ad esempio la riscrittur adi stringhe è  ottenuta imponendo l'associatività  mentre il multiset rewriting aggiungendo anche la commutatività.

## Deduction 

Passando alle deduziione diciamo che la teoria R implica il seguente t t' se e solo se t t' può essere ottenuto attraverso l'applicazione finita di regole di inferenza. 

Le regole di inferenza di base sono queste 4:

1. Abiamo la riflessività, quindi ogni termine implica se stesso 
2. La congruenza ci dice che se abbiamo i seguenti  per tutti i sottotermini allora bbiamo il seguente per il termine composto dal simbolo f applicato ai sottotermini
3. Replacement 
4. Transitivity 

La regola di replacement viene ovviamente semplificata nel caso in cui r sia incondizionata. Non c'è bisogno di una regola generale di sostituzione che puo essere dedotta dalle 4 regole presentate.

## Properties 

Adesso vediamo alcune nozioni e proprietà delle teorie. 

Un sequente derivato dalle regole viene anche detto R-riscrittura concorrente. Questo ci da una prima intuizione dell'equivalenza fra deduzione logica e computazione concorrente presente all'interno del modello.

R si dice terminante se non esistono deduzioni  o catene di riscrittture infinite.

La forma canonica e church rosser vengono descritti nel modo classico. 

## Reflection 

Una proprietà fondamentale per le applicazioni della rewriting logic è la reflection. Ogni teoria T puo essere rappresentata come termine di un'altra teoria U. E poichè la teoria universale U puo essere rappresentata in se stessa possiamo "salire" e "scendere" nei livelli di riflessione. Vedremo quando parleremo di Maude, come questa caratteristica garantisca delle ottime proprietà di metarappresentazione per altri linguaggi e logiche, permettendo un livello avanzato di metaprogrammazione.



## ACT

Ecco, ora ci tengo a delineare quelli che sono i concnetti fondamentali alla base di questa logica. 

Tradizionalmente la riscrittura è definita con una nozione operazionale. Ho una sequenza di riscritture che applicate una dopo l'altra trasformano un termine t in un termine **equivalente** t' (concetto fondamentale). In un certo senso siamo in un mondo statico dove si descrivono equivalenze attraverso determinate equazione. La nozione di riscrittura definita nella rewriting logic è completamente diversa. 



Nella rewriting logic la riscrittura corrisponde esattamente alla deduzione logica.  

La computazione di un termine t'  a partire da t corrsiponde in modo esatto a una deduzione  di una prova da t a t'.

L'altra idea fondamentale è che la concorrenza è implicita nel modello stesso.  e si basa sull'osservazione che molte delle semplificazione possono  essere eseguite in parallelo. 



 L'altra differenza fondamentale è che la rewriting logic NON descrive uguaglianze attraverso la riscrittura  come nella logica eqauzionale. La rewriting logic è una logica per ragionare sui cambiamenti all'interno di un sistema concorrente. Un termine t è una proposizione costruita attraverso i connettivi in SIgma, che afferma che il sistema è in un certo stato  con una certa struttura che puo fare determinate transizioni.

# Sem

Una rewrite theory è una descrizione statica di un sistema. Adesso vediamo il modello semnatico che descrive il comportamento di tale sistema.

Quello che andiamo a cercare è una categoria che chiamamo T_r(X) in cui gli oggetti sono le classi di equivalenza dei termini modulo e e le freccie corrisponodono a R-riscritture concorrenti, ovvero le prove nella logica. Le regole per generare tali termini di prova sono ottenute dalle regole di deduzione andando a decorare i seguenti con termini rapprensentanti le prove. Per semplicità considereremo solo regole non condizionali, ma quello che vedremo viene esteso al caso più generale.



**Regole**



Ciascula regola di generazione definisce un'operazione che, presi termini di prova come argomenti restituisce un nuovo termine di prova. Quello che  viene generato è dunque una struttura algebrica, in particolare il grafo P_r(x) che ha come nodi i termini generati da sigma,e e come freccie le freccie identità, i simboli di funzioni f, le regole r e gli archi dati dalla composizioni di due archi.

La struttura generata  è però ancora puramente sintattica e fa ancora troppe distinzioni che vorremo ignorare. In altre parole, vogliamo andare ad identificare due prove "uguali" rappresentati da diversi termini. Poichè nella rewriting logic le prove corrispondono a computazioni concorrente quello che ci chiediamo è: Quando posso dire che due computazioni concorrenti sono sostanzialmente uguali?

La risposta è completamente assiomatica andando ad identificare prove attraverso determinate equazioni.  Definiamo il modello  T_r(X) come il quoziente di P_R(X) modulo queste equazioni: 

 1. L'associatività e l'identità ci garantiscono che il modello descritto è una categoria.

 2. Ciascun simbolo di funzione f preserva sia composizione che identità. 

 3. Utilizziamo le equazioni E della teoria stessa

    

    Per quanto riguarda la regola di exchange quello che ci dice  è che per ciascuna regola r riscrivere "in cima" attraverso r e riscrivere i sottotermini sono due processi indipendenti che possono essere eseguiti in qualsiasi ordine o anche contemporaneamente identificando sempre le stesse prove e quindi le stesse computazioni.

    Quello che otteniamo è la cosiddetta true concurrency, ovvero identifichiamo le stesse computazioni ignorando la concreta esecuzione che puo avvenire in qualsiasi ordine. 

    Poichè per l'assioma 2 i termini t e t' sono funtori, quello che otteniamo dalla exchange law è che una regola r è una trasformazione naturale. Infatti siccome non importa l'ordine di riscrittura il diagramma in figura commuta e questo ci da proprio la definizione di trasformazione naturale.

    La categoria T_R è soltanto uno dei possibili modelli per la teoria R, il modelo generale è dato dalla nozione di R-system. Un r-system è una categoria S a cui associo anche 

    - una famiglia di funtori fs  per ciascuno f in Sigma tali da soddisfsare le equazioni  in E e 

    - Una trasformazione naturale r_s per ciascuna regola r.

    Per descrivere la categoria degli R-systems si introduce il concetto di R-omomorifsmo che viene definito come un funtore  F tale che preserva le operazioni in f e le regole in r. Quindi per ciasciun simbolo di funzione f quello che abbiamo è un omomorfismo rispetto all'algebra definita da sigma e l'ordine di composizione non cambia il risultato, Per quanto riguarda le regole r "preserva le regole" siginifica che quello che otteniamo è l'identità della trasformazione naturale.
    
    Grazie alla nozione di R-omomorfismo possiamo definire la categoria R-Sys come la categoria che ha come oggetti R-systems e come freccie R-homomorfiismi.
    
    All'interno di questa categoria è possibile provare che l'oggetto iniziale è dato da T_r e l'oggetto libero è T_r(X). Questo è soundness e completness del modello vengono provate in modo dettagliate nel paper originale.  Per la soundness e completness la relazioni di soddisfacibilità e definita dall'esistenza di una trasformazione naturale alpha dal funtore t_s al funtore t'_s.
    
    E possibile definire un R-system anche in modo analogo al lavoro fatto da Lawvere sulle algebre. 
    
    Lawvere aveva scoperto che  data un algebra A definita su Sigma E, l'interpretazione funzionale del terminte t puo rssere esteso a un funtore A cappello da L_t a Set. L_t  sta per Lawvere thory ed è definita come una categoria che a come oggetti i numeri naturali e come freccie le classi di equivalenza di termini t. Prodotto e composizione.Prendendo il prodotto cartesiano come prodotto di set possiamo definire la categoria Mod(Lt, set) come la categoria che ha come oggetti i funtori come A cappello e come freccie trasformazioni naturale. L'assegnazione A -> A cappello si traduce in un isomorfismo fra categorie.
    
    DIscorso per L_R
    
    
    
    Quello che viene formalizzato dagli R-Systems  è l'idea che i modelli di una teoria di riscrittura siano "sistemi", overro  entità che possono essere in  un determinato stato che  può essere cambiato attraverso delle transizioni. Formalmente questo è catturato vedendo il sistema come una categoria dove gli stati sono dati dagli oggetti e le transizioni dalle freccie. Il sistema è reso concorrente dalla presenza di una struttura algebrica aggiuntiva. Stati e transizioni vengono distribuiti secondo questa sturttura algebriica. Ad esempio nelle reti di petri la struttura algebrica  è data dal multiset  dei marking. La ptenza espressiva degli r-systems è inoltre data dalla presenza di transizioni parametrizzate, dette procedure ,  che permettono di modificar elo stato localmente e in modo concorrente. Sono fornite dalle regole di riscritture con variabili che possono essere applicate in tutti gli stati  dove una componente dello stato distirbuito puo istanziare la parte sinistra della regola.  Il fatto che possano essere eseguite in modo concorrente è formaliizato dal concetto di trasformazione naturale.
    
    

# Maude 

Bene, ora possiamo passare alla descrizione dei linguaggi di alto livello, nel nostro caso Maude, basati sulla logica di riscrittura. Utillizare la riscrittura di termini per descrivere algoritmi non è un  concetto nuovo ed è presente in varie forme in molto formalismi come ad esempio il lmaba.calcolo. Quello che cambia è l'interpretazione computazionale. Tradizionalmente la semanticaq di questi linguaggi era basata sulla logica equazionale. Quello che si esegue è una serie di riscritture che permettono di passare da un termine a un altro termine uguale ad esso con un interpretazione puramente funzionale. Nell'interpretazione qui descritta la semnatica è data dalla logica di riscrittura. SI programmano sistemi concorrenti in cui le transizioni sono date da regole di riscrittura concorrenti.  Grazie al modello formale descritto la semantica data è rigorosamente matematica come per i linguaggi basati su logiche equazionali.  

Per dare un esempio, la semantica dei moduli maude viene data dal concetto di macchina iniziale, che unifica semantica operazionale e denotazionale. L'idea intituitiva è che usiamo un sismtema S per calcolare il risultato richiesto in un modello M  appartenente a una sottocategoria di modelli attrraverso un abstraction map.

Bene, passando al linguaggo vero e proprio Maude è un linguaggio dichiarativo  creato all'università dell'illinois e mantenuto da SRI 	international.  Include un sottolinguaggio funzionale basato su un altro linguaggio OBJ3.

Permette di creare programmi per risolvere problemi, specifiche formalli eseguibili di linguaggi e altri modelli o formalismi  ma anche modelli analizzabili e verificabili rispetto a diverse proprietà.



COn le nuove verioni sono state implementate anche  engine per l'unificazione, la soddisfacibilità ma anche theorem prover o moduli per l'I/0. Adesso vediamo le caratteristiche principali del linguaggio attraverso alcuni brevi esempi.



Partiamo dai moduli funzionali con un piccolo esempio che definisce i numeri naturali con l'addizione. La prima cosa dichiarata in un modulo sono i tipi di dati e le operazioni. I tipi sono dichiarati ocn la keyword sort o sorts se ne dichiariamo piu di uno sulla stessa riga. Gli operatori vengono dichiarati con la parola chiave op seguita dal nome dell'operazione due punti la lista dei sort come argumenti freccia  seguita dal sort del risultato. Vengono dichiarati con una sintassi mix fix dove gli underscore indicano la posizione sintattica in cui inserire gli argomenti dell'operatore. Gli operatori come 0 che non prendono  sort in input sono detti costanti. Un operatore puo essere seguito da una lista di attributi tra parentesi quadre.  Vi sono vari tipi di attributi, ad esempio l'attributo ctor sta per constructor e indica che quell'operatore costruisce la forma canonica dei termini della teoria, come ad esempio nei naturali la forma canonica è data da 0 o da s _ dove _ è il termine e s il successore.  Un altro tipo di attributi sono gli attributo equazionali come quelli dichiarati per il +. Questi permettono di dichiarare assiomi equazionali in un modo che permette a maude di usarli in modo efficiente perchè sono implementati in modo builtin dal runtime di maude stesso Ad esempio quelli usati in + ci dicono che è un operatore associativo e commutativo con identità 0. 

Le equazioni sono dichiarate con la keword eq e possono essere condizionali ma nel caso viene utilizzata la keword ceq.

Per i moduli funzionali l'espressioni possono essere valutate col comando reduce o red. Questo comando viene eseguito nella repl di maude che puo essere eseguita con l'ominimo comando

Passiamo ai system modules con un piccolo esempio che descrive un venditore automatico concorrente di torte e mele in cambio di dollari e quarti di dollaro. Posso aggiungere un dollaro o un quarto di dollaro alla macchina oppure comprare una torta per un dollaro o una mela per 75 centesimi. Il sottomodulo funzionale vending machine signature descrive i sort presenti. Notare come i sort coin e item vengono ordinati come subsort del generico marking. Questo perchè in maude i sort possono essere ordinati seguendo una relazione di ordine parziale. I sort vengono raggruppati in classi di equivalenza dette kind che rappresentano le componenti connesse del grafo indotto dalla relazione. Il modulo vending machine include  il sottomodulo funzionale con la keyword including . L'esecuzione puo essere simulata attraverso i comandi rew per rewrite e frew per fair rewrite. La differenza tra i due sta nella strategia di esecuzione, come da nome fair rewrite usa una strategia bottom up dove cerca di  lasciare inutilizzate il minor numero di regole possibili mentre rew procede in modalità top down.

Lo state space puo essere analizzato anche globalmente col comando search, Qui ad esempio possiamo chiedere se esiste un esecuzione che permette di ottenere almeno due torte e una mela partendo da un dollaro e tre quarti. I termini tra parentesi quadre danno un limite inferiore e superiore alla "Profondita della ricerca". Maude restutisce tutti gli stati che soddisfano la condizione imposta.

Passiamo a un esmepio riassuntivo dove definiamo un modulo che descrive due processi concorrenti che vogliono accedere ad una sezione critica regolata da token di accesso. Il modulo mutex definisce la sintassi dei processi che sono visti come coppie formate da un nome che in questo caso puo essere a oppure b e  da una modalità che rappresenta in un certo senso la "posizione del process" che puo essere in attesa o wait oppure in sezione critica rappresentato da critical. Lo stato del sistema è dato dal sort conf che è definito come l'insieme delle configurazioni attuali dei processi. Il token puo assumere valore \$ o \*.  er le regole di entrata Quando il processo a è in attesa e il token ha valore  \$  puo entrare nella sezione critica, viceversa per b quando ha valore *. La regola di uscita semplicemente fa tornare in attesa il processo e assegna il valore al token opposto a quello di entrata.

Per poter fare model checking di questo modulo si definiscono dei predicati da soddisfare nel modulo Mutex-preds. La relazione di soddisfaciblità è defnita nel modulo satisfaction ed è data dall'operatore |=. I due predicati definiti sono crit e wait che applicati a una variabile indicante un processo sono veri se il processo si trova nella corrispondente modalità. Per definire la relazione di soddisfacibilità usiamo una sorta di "parrern matching" dicendo che dato un nome di processo N,  se N è in critical allora crit(n) è vero e stessa cosa per wait(N). Tutti gli altri casi vengono definiti come falsi e vengono riassunti nell'ultimo caso che li racchiude grazie all'attributo owise per otherwise. Questo attributo permette di definire equazioni in modo sintetico andando a defnire l'equazione su tutti i casi non gia definiti dalle equazioni definite in precedenza.

Adesso che abbiamo la descrizione del modello e dei predicati su di esso possiamo verificare la mutua esclusione definendo due stati inziaili a seconda del valore iniziale del token e partendo con entrambi i processi in attesa. Il model checking è definito nel modulo model-checker. La logica utilizzata è quella ltl e la verifica puo essere applicata col comando riportato nell'esempio. La mutua esclusione viene rappresentata dal predicato riportato come secondo argomento di modelCheck il cui significato è non è mai vero che crit(a) e crit(b ) valogno contemporenamente. Riducendo il termine modelcheck vediamo che questa proprietà è soddisfatta in entrambi i casi.

Adesso vediamo com'è definito l'ambinete di programmazione e metaprogrammazione di maude.

L'idea è quella di sfruttare la natura riflessiva della rewriting logic. Ogni modulo M in maude puo essere rappresentano come un termine del Sort module. Questo è fondamentale perchè permette di definire meta programmi in maniera immediata e nativa nel linguaggio con cui ragionare su altri programmi. La reflection è implementata nel modulo META-LEVEL e suoi sottomoduli dove vengono definite funzioni dette descent function che permettono di applicare i comandi e gli operatori nativi al metalivello. Ad esempio meta-apply permette di applicare una regola di un termine in un modulo direttamente al metalivello senza dover "scendere" nel modulo originario.

La presenza di questi moduli e funzioni permette un livello di metaprogrammazione molto avanzato. Ad esempio le strategie di esecuzione sono interne al linguaggio stesso. Un programmatore  puo programmare e comporre strategie fornite dal linguaggio stesso ma anche definirne di nuove attraverso i moduli strategia. 

Maude è ottimo anche come framework dove rappresentare altri modelli formali, logiche o linguaggi. Grazie alle caratteristiche riflessive, queste rappresentazioni sono eseguibili all'interno del sistema stesso, in quanto basta fornire una funzione che trasformi il modulo MOduleL di rappresentazione del linguaggio in un modulo nativo di sort Module (convertendo dunque il modello originale in una teoria di riscrittura).

Queste caratteristiche sono alla base di tutti i comandi di state analysis e simulazione che abbiamo visto e anche per le estensioni che sono state sviluppate come full maude che introduce moduli object oriented o real time maude che introduce transizioni temporizzate. 

Ad esempio vediamo un modulo object oriented che definisce una classe rappresentante un conto in banca. Il conto ha un attributo balance che rappresenta il saldo attuale. I metodi sono rappresentati come messaggi, qui ad esempio vengono definiti  metodi per l'aggiunta e il prelievo di soldi dal conto e un metodo per trasferire una quantità di soldi da un conto a un altro. I metodo vengono implementati da regole con la sintassi riportata dove il metodo a cui fa riferimento al regola è scritto dopo due punti. Gli oggetti sono rappresentati con questa sintassi a "diamante" dove si deifinisce l'identificativo dell'oggetto e lo stato attuale dei suoi attribnuti.

# Google

Ora possiamo passare all'ultima parte dove vediamo un'applicazione concreta della rewriting logic e di maude. In realtà ci sono molte piu applicazioni sviluppate che vengono qui riportate come ad esempio per la descrizione della semantica di un linguaggio di programmazione  o come metalogica nel quale rappresentare ede seguire altere logiche. Noi parleremo di quello in rosso e andremo a vedere come è stato utilizzato maude per formalizzare ed estendere google megastore, un datastore distribuito.
