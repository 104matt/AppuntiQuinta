### **1\. Expecting Professionalism *(Kuppelsalen, Copenhagen)***

🔗 [Guarda il video](https://www.youtube.com/watch?v=BSaAMQVq01E)

**Tema principale:** L'etica, la responsabilità professionale e la disciplina dello sviluppatore software moderno.

* **La responsabilità dello sviluppatore:** Poiché il software muove la società moderna, i programmatori hanno un'enorme responsabilità etica e non devono scaricare la colpa su scadenze o manager.  
* **"We Will Not Ship Shit":** La qualità del codice non è negoziabile. Consegnare codice scarso o pieno di bug danneggia il business e la reputazione.  
* **Saper dire "NO":** Un vero professionista deve saper dire di *no* a scadenze irrealistiche, spiegando i rischi piuttosto che fare promesse irrealizzabili.  
* **"QA Will Find Nothing":** Il team di Quality Assurance (QA) non dovrebbe trovare bug banali; il codice deve arrivare al QA già completamente testato dagli sviluppatori.  
* **Continuous Aggressive Learning:** La popolazione dei programmatori raddoppia circa ogni 5 anni (metà del settore ha meno di 5 anni di esperienza). Per rimanere aggiornati e professionali occorre studiare e praticare costantemente.  
* **Mentoring e lavoro di squadra:** Gli sviluppatori più esperti hanno il dovere di fare mentorship ai più giovani e fare in modo che i membri del team si coprano a vicenda sui progetti.

### **2\. TDD Demonstration: Implementing a Stack in Java**

🔗 [Guarda il video](https://www.youtube.com/watch?v=rdLO7pSVrMY)

**Tema principale:** Dimostrazione pratica del *Test-Driven Development* (TDD) guidando la creazione di una struttura dati (uno Stack in Java) passo dopo passo.

* **Le 3 Regole del TDD:**  
  * Non scrivere codice di produzione se non per far passare un test unitario che fallisce.  
  * Non scrivere più test di quanti bastino a mostrare un fallimento (compresi errori di compilazione).  
  * Non scrivere più codice di produzione di quello strettamente necessario a far passare il test corrente.  
* **Il Ciclo Red-Green-Refactor:**  
  * **Red:** Scrivi un test che descrive un comportamento desiderato e che inizialmente fallisce.  
  * **Green:** Scrivi il codice minimo (anche con valori cablati) per far passare il test.  
  * **Refactor:** Pulisci e migliora la struttura del codice senza rompere alcun test.  
* **Dal Particolare al Generico:** Man mano che i test diventano più specifici ed esaustivi, il codice di produzione diventa via via più generico e flessibile.  
* **Approccio incrementale (esempio dello Stack):**  
  * Si parte dal caso base: uno stack appena creato è vuoto (size \== 0).  
  * Si aggiungono progressivamente casi di push, pop, gestione di stack con un elemento, più elementi, e infine i casi limite (*underflow* e *overflow*).  
* **I test come documentazione e rete di sicurezza:** Un'ampia suite di test unitari automatizzati funge da documentazione viva del sistema e permette di modificare il codice in futuro senza la paura di rompere funzionalità esistenti.

### **3\. Clean Architecture**

🔗 [Guarda il video](https://www.youtube.com/watch?v=G08FxxwPjXE)

**Tema principale:** Come strutturare un'architettura software in modo che sia manutenibile, testabile e indipendente da framework, database e interfacce utente.

* **La Regola delle Dipendenze (*The Dependency Rule*):**  
  1. Le dipendenze nel codice sorgente devono puntare **esclusivamente verso l'interno**, ossia verso i livelli con regole di business a più alto livello di astrazione.  
  2. Il codice interno (logica aziendale) non deve mai conoscere dettagli sul codice esterno (framework, database, UI).  
* **Gli strati dell'architettura (dall'interno verso l'esterno):**  
  1. **Entities:** Oggetti e regole di business aziendali (indipendenti dall'applicazione).  
  2. **Use Cases:** Logica e flussi specifici dell'applicazione.  
  3. **Interface Adapters (Controllers, Presenters, Gateways):** Convertitori di dati tra Use Cases e strumenti esterni.  
  4. **Frameworks & Drivers:** UI, Database, Web, dispositivi I/O (dettagli tecnici di basso livello).  
* **Screaming Architecture ("Architettura d'impatto"):** L'architettura di un progetto deve "urlare" la sua vera natura (es. "Sistema di Gestione Ospedaliera") e non il framework utilizzato (es. "App Spring/Rails/Angular").  
* **Rinviare le decisioni sui dettagli (*Deferring Decisions*):** Una buona architettura permette di ritardare il più possibile scelte tecnologiche secondarie (quale DB usare, quale framework web, ecc.) senza bloccare lo sviluppo della logica di business.  
* **Testabilità e Indipendenza:** La logica di business deve potersi testare in modo rapido e isolato, senza dover avviare un server web, un database o un'interfaccia grafica.

# **APPUNTI 10/10/2026**

# **LA COMPLESSITÀ DELLA GESTIONE AZIENDALE**

Gestire un'azienda è difficile perché chi la guida deve confrontarsi con la **variabilità** di diversi elementi. Ci sono quindi modificazioni **esterne** ed **interne** ad un'azienda.

## **Variabili Esterne** (che si modificano nel tempo)

- **Mercato:** Il mercato cambia (es. cambiano i tassi di interesse).
- **Settori trainanti:** Presenza di un settore che "tira molto" (tipo aziende che si occupano dell'AI).
- **Reputazione dell'azienda:** Se l'azienda fa un errore importante è difficile riprendersi.
- **Rapporto tra consumatori e produttori:** Il consumatore cambia gusto (es. la crisi delle aziende del vino).
- **Trend degli acquisti:** Es. nel settore automobilistico va più il noleggio che la vendita.
- **Normativa e burocrazia:** Cambiano le leggi e gli adempimenti.
- **Fornitori:** Si modificano le dinamiche dei fornitori.
- **Tassazione:** Si modifica la tassazione (es. l'aliquota è cambiata).

## **Variabili Interne**

- **Dipendenti:** Gestione di aspettative, esigenze diverse e welfare aziendale.
- **Azionisti:** Nelle aziende di grandi dimensioni potrebbero esserci dei problemi con gli azionisti.

>[!note] Nota di gestione
>Occorre gestire tutte le variabili (esterne ed interne) che possono presentarsi **insieme**.

# **BISOGNI**

**Definizione:** Si intende uno stato di necessità avvertito da un singolo individuo o da una collettività e che può essere soddisfatto tramite l'uso o il consumo di un **bene** o usufruendo di un **servizio**. L'economia viene messa in moto dal bisogno degli individui. Il bisogno si trova al centro quando si vuole produrre un prodotto o un servizio.

**Il bisogno percepito per un segmento del mercato:** Es. quale bisogno ha soddisfatto FB? Rimanere in contatto con le persone, mettersi in mostra.

## **Caratteristiche dei Bisogni**

- **Soggettività:** Variano da persona a persona.
- **Variabilità nel tempo e nello spazio.**
- **Illimitatezza.**
- **Saziabilità:** Il singolo bisogno si esaurisce una volta soddisfatto attraverso un bene o un servizio.
- **Risorgenza:** Molti bisogni risorgono dopo un po' di tempo (es. bisogno di dormire, di mangiare, ecc.)

## **Classificazione dei Bisogni**

- **Primari:** Legati alla sopravvivenza.
- **Secondari:** Legati alla qualità della vita.
- **Individuali:** Bisogni del singolo.
- **Collettivi:** Es. le strade, la sicurezza, la giustizia, ecc.

# **BENI**

**Definizione:** Il bene è un oggetto disponibile in quantità limitata, perché altrimenti non avrebbe valore economico, ma ovviamente reperibile e utile, cioè idoneo a soddisfare un bisogno.

Possiamo dividere i beni in liberi ed economici:

- **Liberi:** ovvero in quantità illimitata, non possiedono valore economico, non richiedono alcuno sforzo per essere ottenuti, perché sennò avrebbero un valore economico (aria, luce, ecc.)
- **Economici:** Disponibili in quantità limitata, possiedono un valore economico, richiedono un certo sforzo (comportano il pagamento di un prezzo, anche per il reperimento).

Secondo il **criterio della natura** i beni si possono classificare in:

- **Materiali:** minerali, acqua
- **Immateriali (intangibili):** software, loghi/marchi, diritti d'autore

Secondo la destinazione possiamo classificare i beni in beni di **consumo** e beni **strumentali**. Esempio di bene di consumo sono gli spaghetti col sugo, esempio di bene strumentale è un **computer o un martello**.

Secondo il **criterio della durata** i **beni** si possono classificare in beni **durevoli** e beni **non durevoli**. I beni non durevoli si utilizzano tutti in una volta, come gli **spaghetti col tonno** mentre la **macchina** è un **bene durevole** (se usata privatamente è un bene di consumo durevole, se usata per lavorare, es. un tassista, diventa un bene strumentale).

Secondo il criterio della **modalità di utilizzo** possiamo suddividere i **beni** in **beni complementari** o **beni surrogati o succedanei**. I beni **complementari** sono beni che hanno bisogno di altri beni per essere utilizzati, ad esempio la stampante col toner e con la carta. I beni **surrogati o succedanei** sono beni sostituibili l'uno con l'altro, come il **burro** con la **margarina**.

Per soddisfare i bisogni acquistiamo beni o **utilizziamo i Servizi.**

# **SERVIZI**

**Definizione:** Un servizio è un compito svolto da un soggetto **per soddisfare i bisogni di un altro soggetto**, senza che questa attività comporti il trasferimento di proprietà di un bene tangibile (es. il barbiere eroga un servizio, non mi dà un oggetto tangibile).

Le caratteristiche di un servizio sono:

- **Intangibilità:** Non si possono toccare.  
- **Eterogeneità:** I servizi variano a seconda di chi li fornisce e a seconda delle circostanze in cui vengono erogati.  
- **Inseparabilità della produzione dal consumo:** Se mi taglio i capelli, nel momento in cui viene erogato il servizio i miei capelli vengono tagliati. **Non puoi separare la produzione e il consumo di questo servizio.**  
- **Deperibilità:** Non posso immagazzinare i servizi per poterli utilizzare in futuro (conseguenza della caratteristica precedente).

## **Classificazione dei servizi**

Può essere fatta secondo il **Criterio della natura:** Secondo quest’ultimo possiamo distinguere servizi pubblici e servizi privati.

- Pubblici: erogati da enti pubblici, come ASL, Scuola, Tribunali etc…  
- Privati: es. possono esistere scuole private

Secondo il **Criterio del settore** i servizi possono essere divisi in:

- **Terziari**: riguardano turismo, commercio, logistica etc…  
- **Quaternari** **(Detti anche Terziari avanzati)**: che trattano gestione di informazioni, ricerca, sviluppo come l’informatica o la ricerca scientifica (es. nel settore chimico, biologico, ecc.).

Secondo il **tipo di bisogno** suddividiamo i servizi in:

- **Prima necessità**: Gli alloggi, l’hotel è di prima necessità perché abbiamo bisogno di dormire. Questo vale anche per la **ristorazione** e il **primo/pronto soccorso.**  
- **Consumo:** Legati al benessere della persona come l’intrattenimento (come il lunapark, il cinema, il teatro) il turismo e la **ristorazione** *(Che è anche di prima necessità)*.  
- **Produzione:** Servono per produrre altri servizi. Comprende vari servizi come la consulenza aziendale, consulenza fiscale, la logistica e la manutenzione della caldaia.

# **SISTEMI ECONOMICI**

Il sistema economico rappresenta il modello mentale/paradigma che guida le politiche economiche di un paese. Il sistema economico risponde alle domande: **cosa** produrre, **come** produrre e **per chi** produrre. In funzione delle risposte il governante deve scegliere un **modello economico**.

## IL MODELLO CAPITALISTA/LIBERISTA

Il **primo modello** economico che vediamo è quello **capitalista/liberista** di matrice più occidentale: 
- **Libertà di iniziativa**  
- **Proprietà privata**  
- Le uniche cose che vanno in mano allo stato sono la sicurezza interna/ordine pubblico, la giustizia e le forze armate, sennò lo stato fa danni economici alle imprese. Tutto il resto deve essere **privato**.	  
- Si punta a fare le cose in grande  
- Creato dall'inglese **Adam Smith** che crea la legge della Mano invisibile, dove il mercato si **autogestisce** in piena libertà senza l’intervento dello stato.
- Tuttora in **America** questo modello viene ancora adottato in parte, ma non completamente: per esempio ci sono scuole sia pubbliche che private, ma quelle pubbliche non sono come in Italia, e quindi per avere una buona istruzione bisogna andare in una privata.

Questo modello ha creato **forte disparità sociale** e **tensioni sociali**. Partendo dal presupposto che ognuno parte da una classe sociale (che può essere proletaria, elitaria, intellettuale) diversa, bisognerebbe dare a **TUTTI** le **stesse opportunità**. Purtroppo però questo modello economico non fornisce le **opportunità** a coloro che sono più svantaggiati di essere al pari con gli altri a livello economico, di istruzione etc... Se tutto viene privatizzato, ci saranno ad esempio livelli di istruzione diversi in base alle disponibilità economiche.

Un **ricco attira più soldi** solo perché lo è già. Ed essendo diventato ricco una volta, le competenze rimangono, quindi rimarrà sempre superiore al povero che non sapeva fare prima e non sa fare ora.

>[!info] I social spesso influenzano e polarizzano il pensiero politico dei ragazzi senza che essi se ne rendano conto, bisogna stare attenti.
>

Il **1929** mise in **crisi** il **modello capitalista/liberista** con lo scoppio di **una bolla** (il **crollo di Wall Street**) che fu **talmente tragica** da mettere in **crisi** anche gli **economisti**.

## IL MODELLO COLLETTIVISTA

Il secondo **modello** che vediamo è quello **collettivista** di matrice orientale ed è l'altro estremo (Originario Russo, Cinese, Vietnamita):
- Si punta alla parificazione di tutti: tutti devono essere uguali, non per esempio davanti alla legge, ma per come ci si veste, per dove si abita, per quello che si mangia, tutti uguali al 100%. **Non è che vengono date pari possibilità a tutti, ma vengono resi tutti uguali.**
- Non esiste più l'iniziativa privata né la proprietà privata, lo stato ti fornisce tutto quello che è necessario per sopravvivere
	- Non puoi permetterti di discostarti troppo da quello che ti hanno detto di essere, perché sennò inizi ad essere un reazionario, un capitalista. Se provi a fare qualcosa di diverso o di innovativo vieni visto come un deviato, diverso.
	- C'è un'uguaglianza imposta, anche sminuendo gli oppositori

>[!tip] Info aggiuntiva (non detta in classe)
>Il modello collettivista si ispira al pensiero di **Karl Marx** ed è stato applicato soprattutto nell'**URSS** e nei paesi del **blocco dell'Est**, oltre che in Cina e Vietnam. Lo **Stato** decide tramite una **pianificazione centrale** cosa, come e quanto produrre.



Negli **anni '30** ci fu una riflessione dopo le varie crisi economiche. L'economista **Keynes** sviluppò un **nuovo pensiero** misto/ibrido, **tra i due estremi**. Lui nota che **il sistema liberista** non era tutto da buttare via: il problema del sistema capitalista era che quando il mercato tendeva ad andare male **nessuno interveniva** per risolvere. Decreta quindi che:
- Quando il **mercato si espande**, **lo stato deve** effettivamente **farsi da parte**. Quando però si giunge a una depressione economica lo **stato** deve **intervenire** (temporaneamente, senza eliminare mercato e proprietà privata) sul mercato **facendolo ripartire**. Lo stato può **rilanciare il mercato** tramite degli **investimenti strutturali** su opere molto costose che continueranno ad avere una loro utilità finita la crisi (Per Esempio Strade, Ponti, Scuole). Deve **creare** quindi qualcosa che **rimanga nel tempo** e che richieda **molti anni di lavoro**.
	- Per fare questi **investimenti** lo stato **ha bisogno di denaro** e di conseguenza si **indebita** chiedendo al **mercato** il denaro e fornendogli dei **titoli di stato**.
		- Un cittadino può acquistare dei **titoli di debito** dallo stato con la promessa di ricevere in ritorno la stessa somma di denaro **più** un **tasso di interesse** (premio). Tanto più alto sarà il **rischio**, tanto più alto sarà il tasso di interesse.
