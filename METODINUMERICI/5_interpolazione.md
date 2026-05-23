<h1 style="color: red;">Interpolazione di dati e funzioni</h1>

**Definizione:** Siano $(x_i, y_i)$, $i = 0, \ldots, n$ punti fissati nel piano cartesiano, con $x_i \in [a,b]$. Il problema dell'interpolazione consisite nel **costruire una funzione $g: [a,b] \to \mathbb{R}$ il cui grafico passa per i punti dati**, ossia che soddisfa le **condizioni di interpolazione**
$$ g(x_i) = y_i, \quad i = 0, \ldots, n $$
La funzione $g$ è detta **funzione interpolante** dei punti di interpolazione $(x_i, y_i)$ dati. <br>
Le ascisse dei punti di interpolazione $x_i$ si chiamano **nodi**. <br>
Quando le ordinate dei punti di interpolazione sono le immagini dei nodi tramite una funzione $f: [a,b] \to \mathbb{R}$, allora si dice che $g$ **interpola la funzione $f$**. <br>

- Fissati i punti da interpolare, esistono infinite funzioni interpolanti.
- Per avere l'unicità della soluzione del problema dell'interpolazione occorre *restringere la classe di funzioni interpolanti*.

| Dati input | Output | Scopo |
| --- | --- | --- |
| $(x_i, y_i)$, $i = 0, \ldots, n$ $\quad$ | $g: [a,b] \to \mathbb{R}$ con $g(x_i) = y_i$ | $(x_*, g(x_*))$ con $x_* \in [a,b]$ |

### Interpolazione polinomiale
Le funzioni che useremo per interpolare i nodi sono dei polinomi. <br>
> **Definizione:** Un polinomio è una funzione particolare che si scrive come *combinazione lineare di monomi* (ovvero di potenze intere non negative di $x$) con coefficienti reali. <br>
> $$ p(x) = a_0 + a_1 x + a_2 x^2 + \ldots + a_n x^n \Rightarrow \text{polinomio di grado } n $$

> <h2 style="color: red;">Teorema fondamentale dell'algebra</h2>
> 
> Dati $n+1$ punti del piano $(x_i, y_i)$, $i = 0, \ldots, n$, esiste un unico polinomio di grado al più $n$, denotato con $p_n(x)$, che li interpola, ossia tale che 
> $$ p_n(x_i) = y_i, \quad i = 0, \ldots, n $$
> Il polinomio $p_n$ è detto **polinomio di interpolazione** dei punti di interpolazione $(x_i, y_i)$ dati. <br>

Fissati $n$ nodi da interpolare, esiste un unico polinomio di grado al più $n-1$ che li interpola. <br>

**Calcolo del polinomio di interpolazione:** 
- Dal punto di vista numerico "calcolare" il polinomio di interpolazione significa *progettare un algoritmo* che, dati in input i punti di interpolazione $(x_i, y_i)$, $i = 0, \ldots, n$ e il punto $x_*$ in cui si vuole valutare il polinomio, restituisca il valore $p_n(x_*)$.
- A seconda di come si rappresenta il polinomio, ci sono diverse possibilità per individuarlo in modo univoco tramite $n+1$ valori reali. <br>

### Rappresentazione in forma canonica
Le **condizioni di interpolazione** si traducono in un sistema lineare di $n+1$ equazioni in $n+1$ incognite, che sono i coefficienti del polinomio. <br>
$$ \begin{cases} a_0 + a_1 x_0 + a_2 x_0^2 + \ldots + a_n x_0^n = y_0 \\ a_0 + a_1 x_1 + a_2 x_1^2 + \ldots + a_n x_1^n = y_1 \\ \ldots \\ a_0 + a_1 x_n + a_2 x_n^2 + \ldots + a_n x_n^n = y_n \end{cases} $$
In forma matriciale, il sistema si scrive come $V \alpha = y$, dove
$$ V = \underbrace{\begin{bmatrix} 1 & x_0 & x_0^2 & \ldots & x_0^n \\ 1 & x_1 & x_1^2 & \ldots & x_1^n \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ 1 & x_n & x_n^2 & \ldots & x_n^n \end{bmatrix}}_{\text{matrice dei coefficienti}}, \quad \alpha = \underbrace{\begin{bmatrix} a_0 \\ a_1 \\ a_2 \\ \vdots \\ a_n \end{bmatrix}}_{\text{incognite}}, \quad y = \underbrace{\begin{bmatrix} y_0 \\ y_1 \\ y_2 \\ \vdots \\ y_n \end{bmatrix}}_{\text{termine noto}} $$

Le componenti della soluzione sono i coefficienti del polinomio di interpolazione $p_n(x)$ in forma canonica.
- La matrice $V$ è detta **matrice di Vandermonde**. <br>
- Si dimostra che $det(V) = \prod_{0 \leq i < j \leq n} (x_j - x_i)$, quindi $V$ è invertibile se e solo se i nodi $x_i$ sono distinti (unicità della soluzione). <br>

<h3 style="color: red;">Metodo dei coefficienti indeterminati</h3>

Costo computazionale:
 - La soluzione diretta del sistema $V \alpha = y$ con $n+1$ incognite ha costo $O(n^3)$ (fattorizzazione di V)
 - Una volta calcolati i coefficienti $a_0, a_1, \ldots, a_n$, dato un punto $\bar{x} \neq x_i$, $i = 0, \ldots, n$, si vuole calcolare $p_n(\bar{x})$. L'algoritmo più efficiente è il **metodo di Horner**, che ha un costo di $n$ prodotti e somme floating point

> - Vantaggi: i coefficienti $a_0, a_1, \ldots, a_n$ sono facilmente interpretabili e permettono di calcolare il polinomio in qualsiasi punto.
> - Svantaggi: la matrice di Vandermonde è mal condizionata, quindi il metodo dei coefficienti indeterminati è numericamente instabile.

**Nota importante**: in Python, l'algoritmo di Horner è implementato nella funzione `numpy.polyval`, che consente di valutare un polinomio in un punto dati i suoi coefficienti.

<h3 style="color: red;">Rappresentazione nella forma di Lagrange</h3>
In alternativa alla forma canonica, il polinomio interpolante può essere espresso come combinazione lineare di una base dipendente dai nodi, detta **base di Lagrange**.

$$p_n(x) = \sum_{k=0}^n y_k L_k(x)$$

I termini $L_k(x)$ sono polinomi di grado $n$ caratterizzati da una proprietà fondamentale: si annullano in tutti i nodi di interpolazione tranne quello di indice $k$, nel quale valgono 1.
$$L_k(x_j) = \begin{cases} 1 & \text{se } k = j \\ 0 & \text{se } k \neq j \end{cases}$$

La forma esplicita del $k$-esimo polinomio di Lagrange è:
$$L_k(x) = \prod_{\substack{i=0 \\ i \neq k}}^n \frac{x - x_i}{x_k - x_i}$$

* **Indipendenza lineare:** Si dimostra facilmente (valutando i nodi $x_k$) che la base di Lagrange costituisce un insieme linearmente indipendente.

**Calcolo e Costo Computazionale**
Per ottimizzare la valutazione del polinomio in un punto $\bar{x}$ e non ripetere operazioni al numeratore, si precalcola la quantità $\omega_n(\bar{x}) = \prod_{i=0}^n (\bar{x} - x_i)$. 
Sfruttando questo termine, la formula diventa:
$$L_k(\bar{x}) = \frac{\omega_n(\bar{x})}{(\bar{x} - x_k) \prod_{\substack{i=0 \\ i \neq k}}^n (x_k - x_i)}$$

* **Costo:** L'algoritmo richiede $n^2 + n$ prodotti e $n$ divisioni, con un costo computazionale dell'ordine di $\mathcal{O}(n^2)$.
* **Svantaggio principale:** Aggiungere un nuovo punto di interpolazione (aumentando il grado del polinomio) è inefficiente, poiché costringe a ricalcolare da zero l'intera base di Lagrange.

<h3 style="color: red;">Norme di funzioni</h3>

Il concetto di 'somiglianza' o distanza tra funzioni è espresso mediante la definizione di opportune norme di funzioni.

> **Definizione** <br>
> Sia $f:[a,b] \to \mathbb{R}$ una funzione continua. Allora la **norma infinito** di $f$ è definita come
> $$ \|f\|_\infty = \max_{x \in [a,b]} |f(x)| $$
> La distanza in norma infinito tra due funzioni $f$ e $g: [a,b] \to \mathbb{R}$ è definita come $\|f - g\|_\infty$.

Se $\|f - g\|_\infty < \epsilon$, con $\epsilon > 0$, allora il grafico di $g$ si trova in un 'canale' di raggio $\epsilon$ centrato sul grafico di $f$, la funzione $g$ "approssima" bene $f$. <br>

![1779548240022](image/5_interpolazione/1779548240022.png)

<h3 style="color: red;">Errore (o resto) di interpolazione</h3>

 - Ipotesi: $p_n(x)$ è il polinomio di interpolazione relativo a $(x_i, f(x_i)), i = 0, \ldots, n$
 - Obiettivo: studiare la funzione "resto" $R_n(x) = f(x) - p_n(x)$, ed in particolare trovare una stima di $\|R_n\|_\infty$ per *individuare le condizioni per cui* $p_n(x)$ *si può considerare una buona approssimazione* di $f(x)$ in $[a,b]$.

> **Teorema** <br>
> Sia $f \in C^{n+1}([a,b])$, con $x_i \in [a,b], i = 0, \ldots, n$ e sia $p_n(x)$ il polinomio di interpolazione relativo a $(x_i, f(x_i)), i = 0, \ldots, n$. Allora si ha 
> $$ R_n(x) = \frac{\omega_{x_0, \ldots, x_n}(x)} {(n+1)!} f^{(n+1)} (\xi) $$
> dove $\xi \in [a,b]$ e $\omega_{x_0, \ldots, x_n}(x) = (x - x_0)(x - x_1) \cdots (x - x_n)$.

[Dimostrazione slide 299]