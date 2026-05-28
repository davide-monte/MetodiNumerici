<h1 style='color:red'> Introduzione ai contenuti del corso </h1>

**Calcolo Numerico**: elementi di base di Analisi Numerica (matematica applicata) e programmazione in Python (linguaggio ad alto livello).

> **Di cosa si occupa l'Analisi Numerica?** Lo scopo di questa branca della matematica è risolvere problemi matematici mediante un calcolatore. Comprende l'analisi di problemi reali, la loro modellizzazione matematica, lo sviluppo di algoritmi e la loro implementazione.

Un calcolatore è una macchina in grado di *eseguire rapidamente grandi quantità di operazioni aritmetiche*. Tuttavia, esso non elabora entità astratte, ma solo *dati rappresentabili in forma di numeri*, e ha bisogno di una *lista precisa e ordinata di istruzioni*.
Tutto ciò impone una formulazione opportuna dei problemi e una progettazione accurata delle procedure per risolverli.

**ESEMPIO** (Problemi matematici **analitici** e **numerici**):
Data la funzione $f(x) = \sin(x^2)$, calcolarne la derivata.

- **Soluzione analitica (astratta)**: Applicando i teoremi dell'analisi, $f'(x) = 2x \cos(x^2)$. Il dato in ingresso è una funzione $f(x)$ e il risultato è un'altra funzione $f'(x)$.
- **Soluzione numerica (calcolabile)**: Applicando l'analisi numerica, si definisce una procedura che, dato un numero reale $x$, restituisce un numero reale $y$ che approssima bene la derivata prima in quel punto: $y \approx f'(x)$. Il dato in ingresso è un numero, il risultato è un numero.

| Approccio           | INPUT            | OUTPUT            |
| :------------------ | :--------------- | :---------------- |
| **Analitico** | Funzione $f(x)$ | Funzione $f'(x)$ |
| **Numerico**  | Numero $x$      | Numero $y$       |

> **Definizione generale (ALGORITMO):** Una procedura che, a partire da un insieme di dati in ingresso, produce un insieme di risultati in uscita mediante un numero finito di passi definiti in modo univoco. (Dal matematico arabo al-Khuwarizmi).

Negli **algoritmi numerici**, sia i dati di input che di output sono *numeri reali*. I passi sono costituiti unicamente da successioni delle quattro operazioni aritmetiche fondamentali.

---

## Il problema fondamentale: La Memoria Finita

Lo strumento che esegue i calcoli è il calcolatore, il quale ha uno **spazio di memoria finito**. Questo ha conseguenze enormi sulla progettazione e sull'analisi degli algoritmi.

**Esempio pratico (Python):**

```python
u = 1.0
i = 0
while u + 1 > 1:
    u = u / 2
    i += 1
print(u, i) # Output: 5e-324 1074
```

Eseguendo questo codice, l'output non andrà in loop infinito, ma si fermerà stampando **`1.1102230246251565e-16 53`**. **Significa che, ad un certo punto, per il calcolatore l'uguaglianza **$1 + \frac{1}{2^{53}} = 1$** risulta vera**. **Questo accade proprio perché il computer non ha memoria infinita per rappresentare numeri infinitamente piccoli**.

Da qui sorgono le due domande chiave del corso:

1. **In che modo il calcolatore memorizza i numeri interi e reali?**
2. **In che modo esegue le operazioni aritmetiche su di essi?**

<h1 style='color:red'> La rappresentazione dei numeri </h1>

Distinzione fondamentale: il numero è un'**entità astratta** (es. la quantità "sette"), mentre la **rappresentazione** è la sequenza di simboli usata per codificarlo (es. **$7$**, **$(111)_2$**, **$VII$**).

La convenzione standard è la **notazione posizionale** in base **$\beta$** ($\beta > 1$), in cui ogni cifra assume un "peso" diverso a seconda della sua posizione.

| BASE (β)       | SIMBOLI (Insieme S)                                    |
| ------------------------- | ---------------------------------------------------------------- |
| **2** (Binaria)      | **$\{0, 1\}$**                                           |
| **8** (Ottale)       | **$\{0, 1, 2, 3, 4, 5, 6, 7\}$**                         |
| **16** (Esadecimale) | **$\{0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F\}$** |

Un numero naturale **$N$** è espresso univocamente come stringa di cifre: $N = (c_p c_{p-1} \dots c_1 c_0)_\beta$. Il valore quantitativo si ottiene con la scomposizione polinomiale:

$$
N = c_p \beta^p + c_{p-1} \beta^{p-1} + \dots + c_1 \beta^1 + c_0 \beta^0
$$

> **Regola Fondamentale:** All'aumentare della base, aumenta il numero dei simboli ma diminuiscono le cifre necessarie per rappresentare uno stesso valore.

### Cifre Significative

Dato un numero rappresentato come **$c_p c_{p-1} \dots c_1 c_0$** (con **$c_p \neq 0$**):

* **Le **$t$** cifre meno significative**: Sono i pesi delle potenze *più piccole* della base. Si contano da **destra** verso sinistra.
* **Le **$t$** cifre più significative**: Sono i pesi delle potenze *più grandi* della base. Si contano da **sinistra** (partendo dalla prima cifra non nulla) verso destra.

## Rappresentazione dei numeri reali in base **$\beta$**

Un numero reale **$0 \le \alpha < 1$** si rappresenta come:

$$
\alpha = (0.a_1 a_2 \dots a_t \dots)_\beta = a_1 \beta^{-1} + a_2 \beta^{-2} + \dots = \sum_{i=1}^{\infty} a_i \beta^{-i}
$$

Tutto ciò a destra del punto radice rappresenta il peso di una potenza negativa. **Spesso, per valori irrazionali o periodici, serve un numero infinito di cifre**.

### Forma Scientifica Normalizzata

Un generico numero reale **$\alpha$** (parte intera + frazionaria) si può scrivere sempre "raccogliendo" la potenza adeguata della base $\beta^p$:

$$
\alpha = \text{segno}(\alpha) \cdot m \cdot \beta^p
$$

1. **Segno**: $+1$ o $-1$.
2. **Mantissa (**$m$**)** **: Un numero reale **$< 1$**, composto solo da potenze negative della base**.
3. **Esponente (**$p$**)**: Potenza a cui elevare la base $\beta$.

**Se $a_1 \neq 0$** (cioè la prima cifra decimale della mantissa non è zero), si parla di **forma scientifica normalizzata**. *Solo la forma normalizzata garantisce l'unicità della rappresentazione*.

## Conversioni di Base (Algoritmi)

**Per convertire un numero **$\alpha$** decimale in una base **$\beta$**, si divide il numero nelle sue componenti e si applicano due algoritmi separati**:

1. **Algoritmo delle divisioni successive (per la parte intera)**:
   Si divide la parte intera per la base **$\beta$**. **I resti delle divisioni, letti dall'ultimo al primo, compongono le cifre convertite**.
2. **Algoritmo delle moltiplicazioni successive (per la parte frazionaria)**:
   Si moltiplica la parte frazionaria per **$\beta$**. La parte intera risultante diventerà la cifra della rappresentazione. **Si ripete iterando sempre e solo sulla nuova parte frazionaria residua**.

**Esempio di Sintesi (Convertire **$-(25.375)_{10}$** in base 2)**:

* **Segno** **: Negativo (**$-$**)**.
* **Parte intera** **: **$\lfloor | -25.375 | \rfloor = 25$**$\implies (25)_{10} = (11001)_2$**.
* **Parte frazionaria** : **$0.375$** **$\implies 0.375 \times 2 = 0.75$** (Cifra  **0** ); **$0.75 \times 2 = 1.5$** (Cifra  **1** ); **$0.5 \times 2 = 1.0$** (Cifra  **1**)$\implies (.011)_2$.
* **Risultato**: $-(11001.011)_2$.

<h1 style='color:red'> I Numeri di Macchina - Il formato Fixed Point (Virgola Fissa) </h1>

**Il formato Fixed Point alloca uno spazio di memoria prestabilito di **$t+1$** bit per rappresentare i numeri interi**. **Formati standard in C/Python sono ***short* (**$16$** bit) o *long int* (**$32$** bit). Indichiamo con **$fi(N)$** la stringa memorizzata dal calcolatore per rappresentare $N$.

**Regole di Rappresentazione**:

* **Se **$N \ge 0$**:** Si memorizzano esattamente le **$t+1$** cifre meno significative della sua rappresentazione binaria standard, compresi eventuali zeri a sinistra di riempimento.
* **Se **$N < 0$**:** Si calcola la rappresentazione binaria del valore assoluto **$|N|$**, si prendono le **$t+1$** cifre meno significative, e si esegue il **Complemento a 2**.
  * *Come fare il Complemento a 2:* Si scambiano tutti i bit (**$0 \rightarrow 1$** e **$1 \rightarrow 0$**) e poi si somma **$1$** (in aritmetica binaria).

**Esempio (**$t+1 = 16$ bit): Rappresentare **$N = -1235$**.

1. Rappresentazione di **$|1235|$** su 16 bit: `0000010011010011`.
2. Scambio bit: `1111101100101100`.
3. Somma **$1$**: `1111101100101101`$\implies fi(-1235)$.

### Intervallo di Esatta Rappresentazione e Anomalie

Per una determinata allocazione **$t+1$**, l'**intervallo di esatta rappresentazione** è compreso tra **$[-2^t, 2^t - 1]$**.
**Se l'entità astratta esce da questo range, avviene una perdita di informazioni**:

* **Overflow intero:** Se il numero è troppo grande (**$N > 2^t - 1$**).
* **Underflow intero:** Se il numero è troppo piccolo, ossia eccessivamente negativo (**$N < -2^t$**).

> **NOTA BENE:** In entrambi i casi, il calcolatore applica ciecamente la regola, memorizzando *solo* i **$t+1$** bit meno significativi che ci stanno nello spazio allocato.

### Aritmetica dei numeri Fixed Point

Anche il risultato di un'operazione tra interi subisce le stesse regole di memoria. Il calcolatore fa l'operazione in binario standard, ma poi **"salva" solo i $t+1$ bit meno significativi**.

**Esempi limite (Macchina con **$t+1 = 4$** bit, max **$7$**, min **$-8$**)**:

* **Somma in Overflow (**$7 + 4$**)**:
  **$7$** in binario è **$(0111)_2$** e **$4$** è **$(0100)_2$**.
  **$0111 + 0100 = (1011)_2 = 11$** decimale.
  Il calcolatore memorizza **$1011$**. Tuttavia, poiché il primo bit a sinistra è **$1$**, per la macchina in complemento a due esso rappresenta **$-5$**.
  Dunque, al calcolatore,  **$7 + 4 = -5$** .
* **Moltiplicazione in Overflow (**$7 \times 3$**)**:
  **$21$** in decimale corrisponde a **$(10101)_2$** in binario.
  Dovendo tagliare a 4 bit, si prendono i meno significativi: si "scarta" l'1 iniziale, tenendo solo.
  Dunque, per il calcolatore, $7 \times 3 = 5$.
