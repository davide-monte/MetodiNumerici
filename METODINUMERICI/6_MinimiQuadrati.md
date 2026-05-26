<h1 style="color:red;">Approssimazione di dati e funzioni: il criterio dei minimi quadrati</h1>

## 1. Introduzione: Perché non usare l'interpolazione?
Nelle applicazioni pratiche, le misurazioni di un fenomeno (es. rilevare la differenza di potenziale per dedurre la resistenza tramite la legge di Ohm $V=iR$) contengono sempre una certa quantità di **errore**. 
Se provassimo ad approssimare questi dati inesatti tramite l'interpolazione (imponendo che la curva passi *esattamente* per tutti i punti), otterremmo un modello fortemente oscillante e **non significativo** del fenomeno osservato.
L'obiettivo dei minimi quadrati è invece trovare un "modello ideale" (fissato a priori in base alla fisica del problema) che non passi per forza su tutti i punti, ma che ne minimizzi la *distanza globale*.

## 2. Il caso lineare: Retta di regressione
Se sappiamo che il fenomeno ha un andamento lineare, scegliamo il modello:
$$y=a_0+a_1x$$
**Obiettivo:** Trovare i coefficienti (parametri) $a_0$ e $a_1$ che meglio approssimano i dati raccolti $(x_i, y_i)$ per $i=1,\dots,m$.

### Criterio dei minimi quadrati
Si determinano i parametri del modello scelto che **minimizzano** la distanza quadratica cumulativa tra i dati previsti dal modello e quelli misurati:
$$Q(a_0,a_1)=\sum_{i=1}^{m}(a_0+a_1x_i-y_i)^2$$

### Formulazione matriciale della retta di regressione
Introduciamo i vettori delle predizioni e delle misurazioni:
$$q(a_0,a_1)=\begin{bmatrix}a_0+a_1x_1\\a_0+a_1x_2\\\vdots\\a_0+a_1x_m\end{bmatrix}\in\mathbb{R}^{m}\quad\text{e}\quad y=\begin{bmatrix}y_1\\y_2\\\vdots\\y_m\end{bmatrix}\in\mathbb{R}^{m}$$
In questo modo, la funzione da minimizzare diventa la norma al quadrato del vettore residuo:
$$Q(a_0,a_1)=||q(a_0,a_1)-y||^2$$
Separando le variabili dalle incognite, esprimiamo $q(a_0,a_1)$ in forma matriciale come $A\alpha$:
$$A=\begin{bmatrix}1&x_1\\1&x_2\\\vdots&\vdots\\1&x_m\end{bmatrix}\in\mathbb{R}^{m\times 2}\quad\text{e}\quad\alpha=\begin{bmatrix}a_0\\a_1\end{bmatrix}\in\mathbb{R}^2$$
Dunque la funzione da minimizzare diventa esplicitamente:
$$Q(a_0,a_1)=||A\alpha-y||^2$$

---

## 3. Modelli più generali (Lineari nei parametri)
Lo stesso approccio si estende a modelli che dipendono da $n$ parametri $f(a_0,\dots,a_{n-1};x)$. L'obiettivo rimane minimizzare $Q = ||A\alpha - y||^2$.

### Caso Polinomiale
Se scegliamo un modello polinomiale di grado $n-1$:
$$f(a_0,a_1,\dots,a_{n-1};x)=a_0+a_1x+a_2x^2+\dots+a_{n-1}x^{n-1}$$
La matrice $A$ assume la forma (simile a quella di Vandermonde, rettangolare):
$$A=\begin{bmatrix}1&x_1&x_1^2&\dots&x_1^{n-1}\\1&x_2&x_2^2&\dots&x_2^{n-1}\\\vdots&\vdots&\vdots&\ddots&\vdots\\1&x_m&x_m^2&\dots&x_m^{n-1}\end{bmatrix}\in\mathbb{R}^{m\times n}\quad\text{e}\quad\alpha=\begin{bmatrix}a_0\\a_1\\\vdots\\a_{n-1}\end{bmatrix}\in\mathbb{R}^n$$
*Osservazione:* Se $n=m$ (numero di parametri uguale al numero di punti dati), il problema si riduce alla ricerca del classico polinomio di interpolazione.

### Caso Generale Lineare
Più in generale, il modello può essere una combinazione lineare di un insieme di $n$ funzioni di base $\phi_i(x)$:
$$f(a_0,a_1,\dots,a_{n-1};x)=a_0\phi_0(x)+a_1\phi_1(x)+\dots+a_{n-1}\phi_{n-1}(x)$$
In tal caso, la matrice $A\in\mathbb{R}^{m\times n}$ conterrà le funzioni di base valutate nei punti $x_i$:
$$A=\begin{bmatrix}\phi_0(x_1)&\phi_1(x_1)&\dots&\phi_{n-1}(x_1)\\\phi_0(x_2)&\phi_1(x_2)&\dots&\phi_{n-1}(x_2)\\\vdots&\vdots&\ddots&\vdots\\\phi_0(x_m)&\phi_1(x_m)&\dots&\phi_{n-1}(x_m)\end{bmatrix}$$

---

## 4. Calcolo della soluzione del problema ai minimi quadrati
Per trovare i coefficienti ideali, occorre risolvere il problema di minimizzazione:
$$\min_{\alpha\in\mathbb{R}^n}||A\alpha-y||^2$$
Questo problema non si risolve banalmente imponendo le derivate a zero, ma sfruttando opportune fattorizzazioni della matrice $A$. 

### Il Caso Non Degenere ($n \le m$, rango di $A = n$)
Si assume che le $n$ colonne della matrice $A$ siano linearmente indipendenti (come avviene per la regressione polinomiale standard).

**Teorema:** Se $A\in\mathbb{R}^{m\times n}$ ha rango $n$, allora la soluzione al problema dei minimi quadrati è **unica**.
La dimostrazione sfrutta l'invarianza della norma euclidea rispetto alle trasformazioni ortogonali ($||Qx||^2 = ||x||^2$ se $Q$ è ortogonale). Si scompone la matrice tramite fattorizzazione:
$$A=Q\begin{bmatrix}R\\0\end{bmatrix}$$
dove $Q\in\mathbb{R}^{m\times m}$ è ortogonale e $R\in\mathbb{R}^{n\times n}$ è triangolare superiore e non singolare. Si arriva quindi a riformulare la distanza da minimizzare come:
$$||A\alpha-y||^2=||R\alpha-\tilde{y}_1||^2+||\tilde{y}_2||^2$$
Poiché $||\tilde{y}_2||^2$ non dipende da $\alpha$, il minimo si ottiene semplicemente azzerando il primo termine, ovvero ponendo $R\alpha=\tilde{y}_1$. Poiché $R$ è non singolare, l'equazione ha una e una sola soluzione.

### Algoritmo di risoluzione (Procedura pratica)
1. Calcolare la fattorizzazione $A=Q\begin{bmatrix}R\\0\end{bmatrix}$ (generalmente mediante **trasformazioni di Householder**).
2. Calcolare il vettore ruotato $\tilde{y}=Q^T y$.
3. Estrarre le prime $n$ componenti di $\tilde{y}$ e memorizzarle nel vettore ridotto $\tilde{y}_1$.
4. Risolvere il sistema triangolare superiore $R\alpha=\tilde{y}_1$ (molto efficiente tramite l'algoritmo di sostituzione all'indietro) per ottenere i coefficienti ottimali $\alpha$.

### Caso degenere
Se le colonne di $A$ non sono linearmente indipendenti (ossia se il rango $A$ è minore di $n$), il problema dei minimi quadrati ammette infinite soluzioni, che dipendono da $n-k$ parametri liberi, dove $k$ è il rango di $A$. <br>
Nel sottospazio delle soluzioni, si individua quella che ha norma euclidea minima, detta **soluzione di norma minima**. <br>
Per la dimostrazione dell'esistenza e unicità di questa soluzione, che definisce anche l'algoritmo per calcolarla, si utilizza la **fattorizzazione SVD (Singular Value Decomposition)** della matrice $A$.

<h1 style="color:red;">La decomposizione ai valori singolari (SVD)</h1>

Sia $A\in\mathbb{R}^{m\times n}$ una matrice di rango $k$. Allora $A$ si può fattorizzare come:
$$A = U \Sigma V^T$$
dove:
 - $U \in \mathbb{R}^{m \times m}$, $V \in \mathbb{R}^{n \times n}$ sono matrici ortogonali;
 - $\Sigma \in \mathbb{R}^{m \times n}$ è una matrice rettangolare diagonale (con elementi non negativi) che contiene i **valori singolari** $\sigma_1, \sigma_2, \dots, \sigma_k$ di $A$, della forma:
$$\Sigma = \begin{pmatrix}
\sigma_1 &          &        &          & 0 & \dots & 0 \\
         & \sigma_2 &        &          & 0 & \dots & 0 \\
         &          & \ddots &          &   &       &   \\
         &          &        & \sigma_k & 0 & \dots & 0 \\
0        & \dots    & \dots  & 0        & 0 & \dots & 0 \\
\dots    &          &        &          &   &       &   \\
0        & \dots    & \dots  & 0        & 0 & \dots & 0
\end{pmatrix}$$
con $\sigma_1 \geq \sigma_2 \geq \dots \geq \sigma_k > 0 \quad \text{e} \quad \sigma_{k+1} = \dots = \sigma_{\min(m,n)} = 0$.

> I valori $\sigma_i$ sono detti **valori singolari** di $A$. Inoltre, $\sigma_i^2$ sono gli autovalori di $A^TA$, mentre le colonne di $U$ e di $V$ sono gli autovettori di $AA^T$ e $A^TA$, rispettivamente.

---

## 1. Il Problema dei Minimi Quadrati - Caso Generale (Rango non massimo)

Nel caso in cui la matrice $A \in \mathbb{R}^{m \times n}$ abbia rango $k < n$ (caso degenere), il sistema ha **infinite soluzioni** che minimizzano la distanza $||A\alpha - y||^2$. 
Tra tutte queste infinite soluzioni, ce n'è **sempre una ed una sola che ha norma minima** (ovvero la soluzione "più piccola" e stabile possibile). La SVD ci permette di trovarla.

**Teorema:** Se $A \in \mathbb{R}^{m \times n}$ (con $n \le m$) ha rango $k \le n$, allora la soluzione di minima norma del problema $\min_{\alpha \in \mathbb{R}^n} ||A\alpha - y||^2$ è unica.

### Dimostrazione e deduzione intuitiva
Sostituiamo la fattorizzazione SVD all'interno della norma da minimizzare:
$$||A\alpha - y||^2 = ||U\Sigma V^T \alpha - y||^2$$
Ricordando che moltiplicare per una matrice ortogonale ($U^T$) non cambia la norma euclidea di un vettore, moltiplichiamo tutto per $U^T$:
$$= ||U^T(U\Sigma V^T \alpha - y)||^2 = ||\Sigma V^T \alpha - U^T y||^2$$
Per semplificare, facciamo due cambi di variabile:
1. Chiamiamo $z = U^T y$ (il vettore dei dati ruotato).
2. Chiamiamo $\gamma = V^T \alpha$ (il vettore delle incognite ruotato).

Il problema diventa minimizzare $||\Sigma \gamma - z||^2$. Essendo $\Sigma$ diagonale, questa norma si scompone componente per componente:
$$||\Sigma \gamma - z||^2 = \left( \sigma_1\gamma_1 - z_1 \right)^2 + \dots + \left( \sigma_k\gamma_k - z_k \right)^2 + (-z_{k+1})^2 + \dots + (-z_m)^2$$

- Per **minimizzare** questa espressione, dobbiamo annullare i termini dove compare $\gamma$. Scegliamo quindi $\gamma_i = \frac{z_i}{\sigma_i}$ per $i = 1, \dots, k$.
- I valori da $\gamma_{k+1}$ a $\gamma_n$ *non compaiono* nell'espressione (perché moltiplicati per $\sigma=0$), quindi **possono assumere qualsiasi valore in $\mathbb{R}^{n-k}$** (ecco spiegate le infinite soluzioni!).
- Tuttavia, noi cerchiamo la soluzione di **norma minima**. Poiché l'ortogonalità conserva le norme, $||\alpha||^2 = ||\gamma||^2$. Per rendere $||\gamma||$ il più piccolo possibile, l'unica scelta sensata è annullare le componenti libere: **imponiamo $\gamma_{k+1} = \dots = \gamma_n = 0$**.

### Algoritmo pratico per la soluzione di minima norma
Per calcolare concretamente la soluzione ottima $\alpha$:
1. Calcolare la SVD di $A$: $A = U\Sigma V^T$.
2. Calcolare il vettore $z = U^T y$.
3. Costruire il vettore $\gamma \in \mathbb{R}^n$ definendolo come:
   $$\gamma = \begin{pmatrix} z_1/\sigma_1 \\ \vdots \\ z_k/\sigma_k \\ 0 \\ \vdots \\ 0 \end{pmatrix}$$
4. Recuperare la soluzione nel sistema di coordinate originali: $\alpha = V\gamma$.

> **Nota computazionale:** Calcolare la SVD è **molto costoso** (richiede complesse riduzioni bidiagonali e iterazioni). Se sappiamo a priori che la matrice è a rango massimo (caso non degenere, $k=n$), è molto più efficiente usare la fattorizzazione $QR$, che costa "solo" $\frac{n^2(3m-n)}{3}$ operazioni.

---

## 2. La Pseudoinversa di Moore-Penrose

I passaggi visti nell'algoritmo precedente possono essere riassunti in un'unica elegante formula matriciale. Possiamo scrivere:
$$\alpha = V \Sigma^\dagger U^T y$$
Dove la matrice $\Sigma^\dagger \in \mathbb{R}^{n \times m}$ (si legge "Sigma pseudo-inversa") è costruita prendendo la matrice $\Sigma$ trasposta e invertendo i valori singolari non nulli:
$$ \Sigma^\dagger = \begin{pmatrix}
1/\sigma_1 & 0 & \dots & 0 & 0 & \dots & 0 \\
0 & 1/\sigma_2 & \dots & 0 & 0 & \dots & 0 \\
\vdots & \vdots & \ddots & \vdots & \vdots & & \vdots \\
0 & 0 & \dots & 1/\sigma_k & 0 & \dots & 0 \\
0 & 0 & \dots & 0 & 0 & \dots & 0 \\
\vdots & \vdots & & \vdots & \vdots & & \vdots \\
0 & 0 & \dots & 0 & 0 & \dots & 0
\end{pmatrix} $$

Definiamo quindi la **Matrice Pseudoinversa di Moore-Penrose** (indicata con $A^\dagger$):
$$A^\dagger = V \Sigma^\dagger U^T$$
Così che il problema dei minimi quadrati si risolve banalmente con: **$\alpha = A^\dagger y$**.
*(N.B. Se $A$ è una matrice quadrata e invertibile, allora $A^\dagger$ coincide esattamente con la normale matrice inversa $A^{-1}$).*

---

## 3. SVD come Somma di Matrici (Forma Ridotta o "Economy")

Un modo estremamente utile e intuitivo di riscrivere la SVD è immaginarla non come prodotto di 3 matrici, ma come una **somma di matrici di rango 1**.
Siano $u_i$ la i-esima colonna di $U$ e $v_i$ la i-esima colonna di $V$. Poiché le uniche componenti non nulle di $\Sigma$ sono le prime $k$, possiamo scartare le colonne in eccesso e scrivere:

$$A = \sum_{i=1}^k \sigma_i u_i v_i^T = \sigma_1 u_1 v_1^T + \sigma_2 u_2 v_2^T + \dots + \sigma_k u_k v_k^T$$

Ogni termine $u_i v_i^T$ è a sua volta una matrice di dimensione $m \times n$.
Ragionando allo stesso modo per la pseudoinversa, otteniamo:
$$A^\dagger = \sum_{i=1}^k \frac{1}{\sigma_i} u_i v_i^T$$
Da qui capiamo una cosa fondamentale: per calcolare la soluzione di minima norma $\alpha$, **non serve calcolare l'intera matrice quadrata $U$ (di dimensione $m \times m$) o $V$ (di dimensione $n \times n$)**, ma bastano le loro prime $k$ colonne! Questo risparmio di calcolo è noto nei software scientifici come opzione **"economy SVD"**.

---

## 4. Applicazioni Pratiche della SVD

### A. Compressione di Matrici (es. Compressione Immagini)
Poiché la SVD esprime $A$ come somma pesata (dove i "pesi" sono i valori singolari $\sigma_i$), e poiché sappiamo che $\sigma_1 \ge \sigma_2 \ge \dots \ge \sigma_k$, **i primi termini della somma contengono le informazioni più importanti** della matrice, mentre gli ultimi rappresentano dettagli trascurabili (o rumore).
Possiamo troncare la somma fermandoci ad un indice $\bar{k} < k$:
$$A \simeq \bar{A} = \sum_{i=1}^{\bar{k}} \sigma_i u_i v_i^T$$
- **Risparmio di Memoria:** Invece di memorizzare tutti gli $m \times n$ elementi di $A$, ci basta salvare i vettori $u_1 \dots u_{\bar{k}}$, $v_1 \dots v_{\bar{k}}$ e i valori $\sigma_1 \dots \sigma_{\bar{k}}$, occupando solo **$\bar{k}(m+n+1)$ locazioni float**.
- **Errore di approssimazione:** L'errore che commettiamo scartando i termini successivi è calcolabile in modo esatto!
  - In Norma-2 (spettrale): $||A - \bar{A}||_2 = \sigma_{\bar{k}+1}$ (l'errore è esattamente il primo valore singolare escluso).
  - In Norma di Frobenius: $||A - \bar{A}||_F = \sqrt{\sigma_{\bar{k}+1}^2 + \dots + \sigma_k^2}$

### B. Regolarizzazione di Sistemi Malcondizionati
Se dobbiamo risolvere $Ax = b$ con una matrice quadrata molto malcondizionata, significa che il rapporto $\kappa(A) = \sigma_1 / \sigma_n$ è altissimo, ovvero $\sigma_n$ è vicinissimo a 0.
Visto che l'inversa fa uso di $\frac{1}{\sigma_i}$, un $\sigma_n$ piccolissimo farà esplodere numericamente la soluzione, amplificando a dismisura ogni minuscolo errore nei dati $b$.
La soluzione tramite SVD è **scartare** i valori singolari "problematici". Si fissa un $\bar{k} < n$, si costruisce l'approssimazione troncata $\tilde{A}$ e si risolve $\tilde{A}x = b$, ottenendo una soluzione numericamente stabile e molto vicina a quella reale.

### C. Riduzione della Dimensionalità di Dati
La SVD è alla base di tecniche come la PCA (Principal Component Analysis). Se abbiamo una matrice di dati $A$, possiamo trasformarla (tramite proiezione) in un nuovo spazio usando le prime $\bar{k}$ colonne di $U$:
$$AV \rightarrow (\sigma_1 u_1, \sigma_2 u_2, \dots, \sigma_{\bar{k}} u_{\bar{k}})$$
Questo riduce la dimensione della base di dati da $n$ feature a sole $\bar{k}$ feature principali, **rimuovendo la ridondanza** e scartando le colonne che sono linearmente quasi-dipendenti dalle altre.