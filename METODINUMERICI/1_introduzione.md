# Introduzione ai contenuti del corso

**Calcolo Numerico**: elementi di base di Analisi Numerica (matematica applicata) e di programmazione in Python (linguaggio di programmazione ad alto livello, orientato agli oggetti, con una sintassi semplice e chiara).

> **Di cosa si occupa l'Analisi Numerica?** Lo scopo di questa branca della matematica è quello di _risolvere problemi matematici mediante un calcolatore_.

Un calcolatore (1) è una macchina in grado di *eseguire rapidamente grandi quantità di operazioni aritmetiche*, (2) non è in grado di elaborare entità astratte ma solo *dati rappresentati in forma numerica*, (3) deve avere una *lista precisa e ordinata di istruzioni* da eseguire.

L'uso del calcolatore impone una formulazione opportuna dei problemi da risolvere e la progettazione accurata della procedura con cui calcolarle.

**ESEMPIO** (problemi matematici **analitici** e **numerici**):
Data la funzione $f(x) = \sin(x^2)$, calcolarne la derivata.

- **Soluzione analitica (astratta)**: $f'(x) = 2x \cos(x^2)$. In poche parole, il dato del problema analitico è la funzione $f(x)$, mentre la soluzione è un'altra funzione $f'(x)$.
- **Soluzione numerica (calcolabile)**: applicando i teoremi e i principi dell'analisi numerica, si definisce una procedura che, dato un numero reale $x$, restituisce un numero reale $y$ che approssima bene la derivata prima di $f$ in $x$
  $$
  y \approx f'(x)
  $$

|                     | INPUT    | OUTPUT    |
| ------------------- | -------- | --------- |
| **Analitico** | $f(x)$ | $f'(x)$ |
| **Numerico**  | $x$    | $y$     |

Per ottenere una soluzione numerica, occorre fornire al calcolatore i dati e la lista precisa delle operazioni da eseguire a partire da essi, una lista contenente solo operazioni che il calcolatore è in grado di eseguire.

> **Definizione generale** (ALGORITMO): Un algoritmo è una procedura che, a partire da un insieme di dati in ingresso, produce un insieme di risultati in uscita mediante un numero finito di passi definiti in modo univoco.

Negli algoritmi *numerici* i dati in input e le soluzioni in output sono **numeri reali**, mentre i passi da eseguire sono **successioni di operazioni aritmetiche elementari** (addizione, sottrazione, moltiplicazione, divisione), per poter essere eseguiti da un calcolatore.

> **Punto di partenza del corso**:
>
> 1. In che modo il calcolatore memorizza i numeri interi e reali?
> 2. In che modo il calcolatore esegue le operazioni aritmetiche sui numeri interi e reali?

# La rappresentazione dei numeri

**La notazione posizionale** è un sistema di rappresentazione dei numeri in cui il valore di una cifra dipende dalla sua posizione all'interno del numero.
Il numero è un *entità astratta* (ad esempio, quantità "sette" univocamente determinata), la cui *rappresentazione* è una sequenza di simboli (ad esempio, $7$, $(111)_2$, $VII$, ... molteplice a seconda dei criteri di rappresentazione adottati) che codifica il numero stesso.

La convenzione più comune per rappresentare i numeri è quella di utilizzare un sistema di numerazione posizionale in base $b$ (dove $b$ è un intero maggiore di 1), in cui ogni cifra rappresenta una potenza di $b$.

Ad esempio, nel sistema decimale (base 10), il numero $123$ rappresenta:

$$
1 \cdot 10^2 + 2 \cdot 10^1 + 3 \cdot 10^0 = 100 + 20 + 3
$$

| BASE | SIMBOLI (Insieme $S$)                               |
| ---- | ---------------------------------------------------- |
| 2    | ${0, 1}$                                           |
| 8    | ${0, 1, 2, 3, 4, 5, 6, 7}$                         |
| 16   | ${0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F}$ |

L'insieme dei simboli contiene le cifre da $0$ a $b-1$, dove $b$ è la base del sistema di numerazione.

Secondo la definizione formale, ogni numero naturale **$N$** può essere espresso univocamente come una stringa di cifre in una determinata base **$\beta$**:

$$
N = (c_p c_{p-1} \dots c_1 c_0)_\beta
$$

Per determinare il valore quantitativo di **$N$**, si utilizza la **scomposizione polinomiale**. Ogni cifra **$c_i$** viene moltiplicata per la base **$\beta$** elevata alla potenza corrispondente alla sua posizione **$i$** (partendo da **$0$** da destra verso sinistra):

$$
N = c_p \beta^p + c_{p-1} \beta^{p-1} + \dots + c_1 \beta^1 + c_0 \beta^0
$$

#### Componenti della formula:

* **$c_i$ (Cifra):** Il simbolo grafico scelto dall'insieme **$S$**. Per "abuso di notazione", il simbolo viene identificato direttamente con il suo valore numerico.
* **$i$ (Posizione/Indice):** L'esponente della base. Nota che il conteggio parte sempre da **0** (posizione della cifra meno significativa, a destra).
* **$\beta$ (Base):** Il fattore di moltiplicazione che determina il "peso" di ogni posizione.
* **$p$:** L'indice della cifra più significativa (quella più a sinistra), che determina l'ordine di grandezza del numero.

> **Regola Fondamentale:** In ogni base **$\beta$**, il valore di ogni singola cifra **$c_i$** deve sempre essere strettamente minore della base stessa (**$0 \le c_i < \beta$**).

> **Osservazione**: All'aumentare della base, aumenta il numero di simboli necessari per la rappresentazione dei numeri, ma diminuisce la lunghezza della stringa necessaria per rappresentare lo stesso numero, cioè sono necessarie meno cifre.
> Esempio: il numero decimale **$255$** può essere rappresentato in base **$2$** come **$(11111111)_2$** (8 cifre), mentre in base **$16$** come **$(FF)_{16}$** (2 cifre).

### La rappresentazione dei numeri reali < 1 in base $\beta$

Un numero reale $\alpha$, $0 \le \alpha < 1$, può essere rappresentato in base $\beta > 1$ come $\alpha = (0.a_1 a_2 a_3 \dots)_\beta = a_1 \beta^{-1} + a_2 \beta^{-2} + a_3 \beta^{-3} + \dots$ dove "." è il *punto radice* e $a_i$ sono le cifre (simboli) che rappresentano i numeri da $0$ a $\beta - 1$.

Naturalmente, per rappresentare certi valori (numeri periodici, irrazionali, ...) è necessario un numero infinito di cifre.

## Rappresentazione di un qualsiasi numero reale

Un numero reale $\alpha$ si può ottenere come somma della sua parte intera più la sua parte frazionaria, rimettendo tutto insieme, si ottiene la seguente rappresentazione di un qualsiasi numero reale:

$$
\alpha = (a_n a_{n-1} \dots a_1 a_0 . a_{-1} a_{-2} \dots)_\beta = a_n \beta^n + a_{n-1} \beta^{n-1} + \dots + a_1 \beta^1 + a_0 \beta^0 + a_{-1} \beta^{-1} + a_{-2} \beta^{-2} + \dots
$$

### La Rappresentazione dei Numeri Reali (Forma Normalizzata)

Per rappresentare un qualsiasi numero reale **$\alpha$** all'interno di un sistema di calcolo, è necessario "standardizzare" la sua forma separando le cifre significative dall'ordine di grandezza. Questo si ottiene esprimendo il numero come il prodotto di tre componenti:  **segno** , **mantissa** ed  **esponente** .

#### 1. Il procedimento matematico (Raccoglimento)

Sia dato un numero reale **$\alpha$** in base **$\beta$**, avente **$p$** cifre nella sua parte intera.

La sua espressione polinomiale iniziale è:

$$
\alpha = \text{segno}(\alpha) \cdot (a_1 \beta^{p-1} + a_2 \beta^{p-2} + \dots)
$$

L'obiettivo è spostare idealmente la "virgola" tutta a sinistra. Matematicamente, questo si ottiene  **raccogliendo a fattor comune **$\beta^p$**** .

Dividendo tutti i termini per **$\beta^p$** (il che equivale a sottrarre **$p$** agli esponenti), otteniamo solo potenze negative:

$$
\alpha = \text{segno}(\alpha) \cdot (a_1 \beta^{-1} + a_2 \beta^{-2} + a_3 \beta^{-3} + \dots) \cdot \beta^p
$$

Che può essere scritta in forma compatta come:

$$
\alpha = \text{segno}(\alpha) \cdot \sum_{i=1}^{\infty} (a_i \beta^{-i}) \cdot \beta^p
$$

Ottenendo infine la forma canonica:

$$
\alpha = \text{segno}(\alpha) \cdot m \cdot \beta^p
$$

#### 2. Le tre componenti fondamentali

Dalla formula finale **$\alpha = \text{segno}(\alpha) \cdot m \cdot \beta^p$** emergono i tre "blocchi" con cui un computer memorizza i numeri reali (es. standard *IEEE 754* per i  *float/double* ):

* **Segno:** Vale **$+1$** o **$-1$** e indica se il numero è positivo o negativo.
* **Mantissa (**$m$**):** È la sequenza di tutte le cifre del numero, scritte dopo la virgola. Essendo formata solo da potenze negative della base (**$\beta^{-1}, \beta^{-2}, \dots$**), la mantissa è sempre **un numero reale strettamente minore di 1** (forma **$0,a_1 a_2 \dots$**).
* **Esponente (**$\beta^p$**):** Indica l'ordine di grandezza del numero. Il valore **$p$** (un numero intero) comunica di quante posizioni è necessario spostare la virgola verso destra per ricostruire il numero originale.

> **Esempio pratico in base 10:**
>
> Vogliamo rappresentare il numero **$345,67$**
>
> * Ha **$3$** cifre intere, quindi **$p = 3$**.
> * Raccogliamo **$10^3$**: la virgola si sposta di tre posti a sinistra.
> * La formula diventa: **$+1 \cdot 0,34567 \cdot 10^3$**
>   * Segno: **$+1$**
>   * Mantissa (**$m$**): **$0,34567$**
>   * Esponente: **$10^3$**

Le rappresentazioni del tipo *segno* - *mantissa* - *esponente* si dicono in **forma scientifica** e, se il primo numero decimale della mantissa è diverso da zero, si dicono in **forma scientifica normalizzata**.

> Esempi:
>
> - $(372)_{10}$ (mista)
> - $.372 \times 10^3$ (normalizzata)
> - $.0372 \times 10^4$ (scientifica)

### Algoritmo delle divisioni successive

Serve per rappresentare un numero intero positivo $\alpha \in \mathbb{N}$ da base $10$ ad una diversa base $\beta$.
L'algoritmo consiste nell'eseguire la divisione intera (quoziente e resto) del numero $\alpha$ per la base $\beta$, e continuare a dividere il quoziente ottenuto fino a quando non si ottiene un quoziente nullo.

> **Esempio**: rappresentare il numero decimale **$(1972)_{10}$** in base **$2$**.
>
> 1. $1972 \div 2 = 986$ (quoziente), resto $0$ (cifra meno significativa)
> 2. $986 \div 2 = 493$ (quoziente), resto $0$
> 3. $493 \div 2 = 246$ (quoziente), resto $1$
> 4. $246 \div 2 = 123$ (quoziente), resto $0$
> 5. $123 \div 2 = 61$ (quoziente), resto $1$
> 6. $61 \div 2 = 30$ (quoziente), resto $1$
> 7. $30 \div 2 = 15$ (quoziente), resto $0$
> 8. $15 \div 2 = 7$ (quoziente), resto $1$
> 9. $7 \div 2 = 3$ (quoziente), resto $1$
> 10. $3 \div 2 = 1$ (quoziente), resto $1$
> 11. $1 \div 2 = 0$ (quoziente), resto $1$ (cifra più significativa)
>     Il numero binario è ottenuto leggendo i resti in ordine inverso: **$(11110110100)_2$**.

### Algoritmo delle moltiplicazioni successive

Serve per rappresentare un numero reale $\alpha \in [0, 1)$ in una base $\beta > 1$.
Si tratta di moltiplicare il numero $\alpha$ per la base $\beta$ e prendere la parte intera del risultato come cifra successiva della rappresentazione. Si continua a moltiplicare la parte frazionaria ottenuta fino a quando non si ottiene una parte frazionaria nulla o si raggiunge un numero sufficiente di cifre.

> **Esempio**: rappresentare il numero decimale **$(0.1)_{10}$** in base **$2$**.
>
> 1. $0.1 \times 2 = 0.2$ (parte intera $0$, cifra più significativa)
> 2. $0.2 \times 2 = 0.4$ (parte intera $0$, seconda cifra)
> 3. $0.4 \times 2 = 0.8$ (parte intera $0$, terza cifra)
> 4. $0.8 \times 2 = 1.6$ (parte intera $1$, quarta cifra)
> 5. $0.6 \times 2 = 1.2$ (parte intera $1$, quinta cifra)
> 6. $0.2 \times 2 = 0.4$ (parte intera $0$, sesta cifra)
>    ...
>    $0.1$ in base $2$ è rappresentato come **$(0.0001100110011\ldots)_2$**, un numero periodico.

### Conversione di un numero reale da base 10 a base $\beta$

Ogni numero reale si esprime come somma della sua **parte intera** e della sua **parte frazionaria**. Per effettuare la conversione si combinano gli algoritmi delle divisioni e delle moltiplicazioni successive.

**Procedimento in 4 passi:**

1. Determinare il valore assoluto $|\alpha|$ e memorizzare il **segno**.
2. Isolare la **parte intera** $\lfloor|\alpha|\rfloor$ e convertirla utilizzando l'algoritmo delle *divisioni successive*.
3. Isolare la **parte frazionaria** $|\alpha| - \lfloor|\alpha|\rfloor$ e convertirla utilizzando l'algoritmo delle *moltiplicazioni successive*.
4. **Comporre il numero**: Scrivere il segno, seguito dalla parte intera convertita, il punto radice (la virgola), e infine la parte frazionaria convertita.

---

**Esempio Pratico**
Convertire $\alpha = -(25.375)_{10}$ in base $2$.

1. **Valore assoluto e segno:** $|\alpha| = 25.375$  |  segno = `-`
2. **Conversione parte intera (divisioni successive):** $\lfloor|\alpha|\rfloor = 25 \implies (25)_{10} = (11001)_2$
3. **Conversione parte frazionaria (moltiplicazioni successive):** $|\alpha| - \lfloor|\alpha|\rfloor = 0.375 \implies (0.375)_{10} = (.011)_2$
4. **Risultato finale:** $\alpha = -(11001.011)_2$

---

## Rappresentazione in formato fixed point

### Rappresentazione fixed point degli interi - virgola fissa

Il formato **fixed point** dipende da un parametro $t$:

- viene riservato in memoria uno spazio di $t+1$ bit (formati standard $t+1=16$ (short), $t+1=32$ (long int))
- indichiamo con $fi(N)$ la stringa di cifre binarie ottenuta applicando le regole del formato all'intero $N$. 
  $$
  fi(N) = c_{t} c_{t-1} \ldots c_{1} c_{0}
  $$
- se $N\geq 0$: si memorizzano le $t+1$ cifre meno significative, $d_{t}, d_{t-1}, \ldots, d_{1}, d_{0}$, della sua rappresentazione binaria (eventuali zeri a sinistra compresi)
- se $N<0$: si prendono le $t+1$ cifre meno significative, $d_{t}, d_{t-1}, \ldots, d_{1}, d_{0}$, della rappresentazione binaria di $|N|$ (eventuali zeri a sinistra compresi) e si calcola la rappresentazione in **complemento a 2**.

---

**Esempi ($t+1=16$)**:

- $N = 1235 = (10011010011)_2$ &rArr; $fi(N) = (0000010011010011)_2$ (registrazione in memoria)
- $N = -1235 = -(10011010011)_2$ &rArr; $fi(N) = (1111101100101101)_2$ (registrazione in memoria)
  1. Rappresentazione binaria di $|N|$: $(0000010011010011)_2$
  2. Scambio dei bit: $(1111101100101100)_2$
  3. Aggiunta di 1: $(1111101100101101)_2$

---

**Vedere SLIDE 38** - Errori di rappresentazione
Per un generico t l'intervallo di numeri rappresentabili è $[-2^t, 2^t - 1]$. Se $N$ è al di fuori di questo intervallo, si verifica un **overflow** (il numero non può essere rappresentato correttamente in memoria). In questo caso, il calcolatore memorizza solo le cifre meno significative, causando un errore di rappresentazione.

> **Esempi** con $t+1=4$:
>
> - $N=-9$ &rArr; $fi(N) = (0111)_2$ (registrazione in memoria, ma rappresenta $7$ invece di $-9$)
> - $N=23$ &rArr; $fi(N) = (0111)_2$ (registrazione in memoria, ma rappresenta $7$ invece di $23$)
> - $N=-25$ &rArr; $fi(N) = (0111)_2$ (registrazione in memoria, ma rappresenta $7$ invece di $-25$)

Regola: Si prendono sempre le $t+1$ cifre meno significative della rappresentazione binaria del numero, se è positivo, o della sua rappresentazione in complemento a 2, se è negativo.

- Il caso in cui la perdita di informazione si ha nella rappresentazione di un *intero troppo piccolo*, ossia $N < -2^t$, è detto **underflow intero** (es. $-9, 25$ con $t+1=4$)
- Il caso in cui la perdita di informazione si ha nella rappresentazione di un *intero troppo grande*, ossia $N > 2^t - 1$, è detto **overflow intero** (es. $23$ con $t+1=4$)
- L'intervallo $[-2^t, 2^t-1]$ è chiamato **intervallo di esatta rappresentazione degli interi** (es. $[-8, 7]$ con $t+1=4$)

---
