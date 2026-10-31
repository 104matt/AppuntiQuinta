
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




# FUNZIONI COME VALORI

```cs
Func<T, TResult> prende T e restituisce TResult
Func<T1, T2, R> due parametri fino a 16
Action<T> prende T e non restituisce nulla
Predicate<T> prende T, restituisce bool
```

| FIrma             | Tipo C#             | Esempio              |
| ----------------- | ------------------- | -------------------- |
| int -> string     | Func<int, string>   | i => i.ToString()    |
| () -> string      | Func< string >      | () => "ciao"         |
| int -> ()         | Action< int >       | i = Print(i)         |
| () -> ()          | Action              | () => Print("Ciao!") |
| (int, int) -> int | Func<int, int, int> | (a, b) => a +b       |

### LA FIRMA DI UNA HIGH ORDER FUNCTION

Funzioni che **ricevono** funzioni e **restituiscono** funzioni.

```cs
static R Connetti<R>(string connStr, Func<IDbConnection, R> f)

// freccia: (string, (IDbConnection => R)) -> R
// C#: Func<string, Func<IDbConnection, R>, R>
```

### ORDINARE SENZA DISTRUGGERE

Metti screenshot


### I RECORD

```cs
public record Studente(
	string Nome, string Classe, double Media);
```


## LINQ

C# non nasce come linguaggio funzionale puro ma piano piano si sta integrando. La cosa **piu funzionale** che ha C# è **LINQ**.

Serve per interrogare ogni collezione che implementa l'interfaccia IEnumerableT

Tutti i tipi del **LINQ** vanno importati con  l'import **using system.Linq**. Le due unità base sono le **sequenze**, che e definiamo come sequenza qualsiasi oggetto che implementa IEnumerableT, e gli elementi che sono ciascun **item** nella sequenza

Un **query operator** prende una sequenza in input e ne produce una di output. Dentro LINQ sono contenuti query ioeratir e vengono implementati come metodi statici di estensione (standard query operator).

Una query è un'espressione che trasforma sequenze con query operator.

![](Pasted%20image%2020261021112515.png)


![](Pasted%20image%2020261021113247.png)


![](Pasted%20image%2020261021113327.png)


![](Pasted%20image%2020261021113504.png)


![](Pasted%20image%2020261021113851.png)