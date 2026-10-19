
# PROGRAMMAZIONE FUNZIONALE
Serve a:
- Le funzioni vengono utilizzate come **valori di prima classe** o piu volgarmente come **cittadini di valore**. 
- Immutabilità, se voglio ordinare una lista ne genero una nuova senza mutare quella che sto cercando di ordinare **generando cose nuove.** 


Rispetto alla programmazione orientata agli oggetti, potremmo definirla ortogonale. In un contesto dove la programmazione è orientata agli oggetti e imperativa la programmazione funzionale rimuove la sequenzialità e la ridondanza del codice.

Delle liste che vengono istanziate e che **non vengono** utilizzate se ne occupa il servizio **Garbage Collector** che controlla se queste liste nella **heap** hanno un puntatore col loro indirizzo e se non lo trovano le **cancellano**.

La programmazione funzionale è come una **grande espressione matematica** da ridurre.
### LAMBDA

La Lambda si legge come:

x => x * x; La parte a **sinistra** sono i parametri , la parte a **destra** l'**operazione** da fare
### DELEGATE

Parola chiave, è un tipo che **punta** a una funzione. Esprime la firma di una funzione.

	esempio: public delegate double MathFunction(double x);
## LINQ

C# non nasce come linguaggio funzionale puro ma piano piano si sta integrando. La cosa **piu funzionale** che ha C# è **LINQ**.




