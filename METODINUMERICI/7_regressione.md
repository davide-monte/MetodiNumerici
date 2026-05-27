<h1 style="color:red;">Regressione in più dimensioni</h1>

In molti problemi reali, vogliamo prevedere il valore di una variabile dipendente partendo da molteplici variabili indipendenti.

**Dati a disposizione:** Abbiamo a disposizione un set di dati di addestramento composto da $m$ campioni: ${\{(x^{(i)}, y_i)\}}_{i=1}^m$.
* $x^{(i)} \in \mathbb{R}^p$: è un vettore le cui $p$ componenti sono chiamate **features** (caratteristiche o variabili indipendenti) dell'$i$-esimo campione.
* $y_i \in \mathbb{R}$: è il valore da prevedere, chiamato **risposta** (o target) associata all'$i$-esimo campione.

**Obiettivo:** Fissato un modello parametrico descritto da una funzione $f: \mathbb{R}^p \to \mathbb{R}$, dipendente da un set di parametri $\alpha_0, \ldots, \alpha_n$, vogliamo trovare i valori ottimali di tali parametri affinché il modello approssimi il più fedelmente possibile i dati reali:
$$f(\alpha_0, \ldots, \alpha_n; x^{(i)}) \approx y_i \quad \text{per } i=1, \ldots, m$$

> ### Criterio dei minimi quadrati
> Per quantificare l'errore del nostro modello, si utilizza la somma dei quadrati delle differenze (chiamati *residui*) tra il valore predetto e il valore reale. I parametri ideali sono quelli che minimizzano questa quantità:
> $$\min_{\alpha \in \mathbb{R}^{n+1}} \sum_{i=1}^{m} (f(\alpha_0, \ldots, \alpha_n; x^{(i)}) - y_i)^2$$

---

### Esempi di problemi multidimensionali

**1. Predizione del prezzo di una casa**
Vogliamo prevedere il prezzo in base a $p$ caratteristiche (features):
 - $y_i$: prezzo di vendita della casa $i\text{-esima}$ (**risposta**)
 - $x_1^{(i)}$: area della casa $i\text{-esima}$ (**feature 1**)
 - $x_2^{(i)}$: numero stanze della casa $i\text{-esima}$ (**feature 2**)
 - $x_3^{(i)}$: numero di bagni della casa $i\text{-esima}$ (**feature 3**)
 - $x_4^{(i)}$: 1 se in condominio, 0 se indipendente (**feature 4**, *variabile booleana*)
 - $x_5^{(i)}$: posizione (**feature 5**)

**2. Predizione del voto medio annuale di uno studente**
Un dataset ancora più complesso, con $m = 649$ studenti (campioni) e ben $p = 30$ features per ognuno. Le features includono variabili binarie (es. sesso, accesso a internet), numeriche (es. età, numero di assenze, tempo di studio) e nominali (es. lavoro dei genitori). 
La risposta $y_i$ è la media dei voti finali in matematica (da 0 a 20).

---

### Modello Lineare

Nel caso in cui assumiamo che la funzione $f$ sia una combinazione lineare delle sue features, poniamo il numero di parametri $n = p$ (un parametro per ogni feature, più l'intercetta $\alpha_0$). L'equazione del modello diventa:
$$f(\alpha_0, \ldots, \alpha_p; x^{(i)}) = \alpha_0 + \alpha_1 x_1^{(i)} + \alpha_2 x_2^{(i)} + \ldots + \alpha_p x_p^{(i)}$$

Possiamo esprimere il problema in forma matriciale compatta. Definiamo la **matrice di regressione $A$** (di dimensione $m \times (p+1)$) e il **vettore delle risposte $y$** (di dimensione $m \times 1$):

$$A = \begin{bmatrix} 1 & x_1^{(1)} & x_2^{(1)} & \ldots & x_p^{(1)} \\ 1 & x_1^{(2)} & x_2^{(2)} & \ldots & x_p^{(2)} \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ 1 & x_1^{(m)} & x_2^{(m)} & \ldots & x_p^{(m)} \end{bmatrix} \quad \text{e} \quad y = \begin{bmatrix} y_1 \\ y_2 \\ \vdots \\ y_m \end{bmatrix}$$

> 💡 **Nota intuitiva:** Perché la matrice $A$ ha una colonna di tutti $1$ all'inizio? Quella colonna serve ad essere moltiplicata per il parametro $\alpha_0$ (l'intercetta, o *bias*). In questo modo, il prodotto riga per colonna $A\alpha$ ricostruisce esattamente l'equazione del modello lineare per ogni campione.

Inserendo questa formulazione matriciale, il criterio dei minimi quadrati assume la sua forma lineare classica: 
$$\min_{\alpha \in \mathbb{R}^{p+1}} \|A\alpha - y\|^2$$

---

<h1 style="color:red;">Problemi di regressione non lineare e ottimizzazione numerica</h1>

### Minimi quadrati non lineari

Non sempre una relazione lineare è sufficiente per descrivere la realtà. Se assumiamo che esista una funzione $f: \mathbb{R}^p \to \mathbb{R}$ dipendente da parametri $\alpha_0, \ldots, \alpha_{n-1}$ tale che:
$$f(\alpha_0, \ldots, \alpha_{n-1}; x^{(i)}) \approx y_i \quad \text{per } i=1, \ldots, m$$
L'obiettivo rimane trovare i parametri che minimizzano la funzione residuo:
$$F(\alpha) = \sum_{i=1}^{m} (f(\alpha_0, \ldots, \alpha_{n-1}; x^{(i)}) - y_i)^2$$

> ⚠️ **Concetto Chiave per l'Esame:** Si parla di modello "non lineare" quando la funzione $f$ NON dipende linearmente dai **parametri** $\alpha$, indipendentemente da come vi compaiono le $x$.
> * $f(x) = \alpha_0 + \alpha_1 \sin(x)$ è un modello **lineare** (perché $\alpha_0$ e $\alpha_1$ hanno grado 1).
> * $f(x) = \alpha_0 e^{\alpha_1 x}$ è un modello **non lineare** (perché $\alpha_1$ si trova all'esponente).

Se il modello è non lineare rispetto ai parametri, la funzione da minimizzare $F(\alpha)$ non può essere riscritta nella comoda forma algebrica matriciale $\|A\alpha - y\|^2$.
Di conseguenza, il problema non ammette quasi mai una soluzione esatta tramite risoluzione di sistemi lineari (equazioni normali), e diventa obbligatorio **ricorrere a metodi di ottimizzazione numerica iterativi** (es. Metodo del Gradiente, Gauss-Newton, Levenberg-Marquardt) per approssimare il minimo della funzione $F(\alpha)$.

### Esempi di Modelli Non Lineari: Dinamica delle Popolazioni

Un classico esempio di regressione non lineare riguarda lo studio della crescita di una popolazione nel tempo (ad esempio, il numero di antilopi in un parco africano o l'evoluzione della popolazione italiana dai censimenti ISTAT storici).
In questi casi, la feature $x$ è il tempo (es. anni) e la risposta $y$ è il numero di individui.

Esistono due modelli principali per descrivere questo fenomeno:

**1. Il Modello Esponenziale**
Il modello più semplice assume che il tasso di crescita della popolazione sia costante nel tempo. La funzione parametrica è:
$$f(\alpha_0, \alpha_1; x) = \alpha_0 e^{\alpha_1 x}$$
L'obiettivo è minimizzare la funzione residuo $F(\alpha_0, \alpha_1) = \sum_{i=1}^{m} (\alpha_0 e^{\alpha_1 x_i} - y_i)^2$ trovando i parametri ottimali $\alpha_0$ e $\alpha_1$.
*Limiti del modello:* Nella realtà, nessuna popolazione cresce all'infinito; le risorse limitate causano una stabilizzazione della crescita dopo un certo periodo.

**2. Il Modello Logistico**
Per superare i limiti dell'esponenziale, si utilizza la **funzione logistica**, che è molto più realistica e presenta un caratteristico grafico a forma di "S". 
Questo modello dipende tipicamente da tre parametri:
$$f(\alpha; x) = \frac{\alpha_0}{1 + e^{-\alpha_1(x - \alpha_2)}}$$
> 💡 **Nota intuitiva sui parametri del modello logistico:**
> * **$\alpha_0$**: Rappresenta il "tetto massimo" (o *carrying capacity*), ovvero il limite a cui la popolazione si stabilizza.
> * **$\alpha_1$**: Determina la ripidità della curva (quanto velocemente la popolazione cresce nella fase centrale).
> * **$\alpha_2$**: Rappresenta il punto di flesso, ovvero l'istante temporale $x$ in cui la crescita è massima prima di iniziare a rallentare.

---

<h1 style="color:red;">Il Problema di Ottimizzazione Numerica</h1>

Sia nei problemi di minimi quadrati non lineari, sia in altri contesti come la classificazione, l'obiettivo finale si riduce sempre alla minimizzazione di una determinata funzione, chiamata **funzione obiettivo** $F$.
Questo processo prende il nome di **ottimizzazione numerica non lineare** (o programmazione matematica non lineare).

Matematicamente, si formula come:
$$\min_{\alpha \in \mathbb{R}^n} F(\alpha) \quad \text{con } F: \mathbb{R}^n \to \mathbb{R}$$

### Minimi Locali e Minimi Globali

Quando cerchiamo il "minimo" di una funzione, dobbiamo distinguere due concetti:

* **Punto di minimo locale:** Un punto $\alpha^* \in \mathbb{R}^n$ è un minimo locale se è il punto più basso *solo nelle sue immediate vicinanze*. Formalmente, se esiste un raggio $\epsilon > 0$ tale che $F(\alpha^*) \le F(\alpha)$ per ogni $\alpha$ all'interno della palla $\{z \in \mathbb{R}^n : \|z - \alpha^*\| \le \epsilon\}$.
* **Punto di minimo globale:** Se la disuguaglianza $F(\alpha^*) \le F(\alpha)$ è valida per *tutti* i punti $\alpha \in \mathbb{R}^n$, allora $\alpha^*$ è il minimo globale assoluto della funzione. L'obiettivo ideale dell'ottimizzazione è sempre trovare il minimo globale.

### Equivalenza tra Problemi di Minimo e Massimo

I problemi di minimizzazione e massimizzazione sono due facce della stessa medaglia. Trovare il minimo di una funzione $F(\alpha)$ equivale esattamente a trovare il massimo della stessa funzione ribaltata (cambiata di segno):
$$\min_{\alpha \in \mathbb{R}^n} F(\alpha) \iff \max_{\alpha \in \mathbb{R}^n} -F(\alpha)$$
Le coordinate dei punti ottimali sono le stesse, mentre i valori della funzione $F$ nei punti di ottimo coincidono in valore assoluto ma avranno segno opposto.

---

### Condizioni Necessarie di Ottimalità

Come facciamo a sapere di aver trovato un punto di minimo? In una sola dimensione ($F: \mathbb{R} \to \mathbb{R}$), ci viene in soccorso il calcolo differenziale.

> ### Teorema di Fermat
> Se una funzione $F$ è differenziabile con continuità e $\alpha^* \in \mathbb{R}$ è un suo punto di minimo locale, allora la sua derivata prima in quel punto si annulla:
> $$F'(\alpha^*) = 0$$

**Conseguenze fondamentali per i Metodi Numerici:**
1.  **La ricerca:** Il problema di cercare i punti di minimo di una funzione $F$ si può tradurre (e spesso risolvere) come il problema della ricerca degli zeri della sua derivata prima $F'$. Tutti i punti di minimo devono obbligatoriamente soddisfare l'equazione $F'(\alpha) = 0$.
2.  **Il viceversa NON è sempre vero:** Sapere che $F'(\alpha) = 0$ è una condizione *necessaria ma non sufficiente* (il punto trovato potrebbe essere un massimo o un flesso/sella). La certezza che un punto a derivata nulla sia effettivamente un punto di minimo (condizione sufficiente) è garantita solo sotto ipotesi più stringenti, ad esempio se sappiamo a priori che la funzione $F$ è **convessa**.