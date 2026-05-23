# Schema di Ripasso: Calcolo Matriciale ed Elementi di Algebra Lineare

## 1. Definizioni Fondamentali e Memoria

* **Matrice:** Un array di elementi **$A \in \mathbb{R}^{m \times n}$**, dove **$m$** indica il numero di righe e **$n$** il numero di colonne.
* **Casi specifici:**
  * **Se** $m = n$, la matrice è detta quadrata di ordine **$n$**.
  * **Se** $n = 1$, si ottiene un vettore colonna (spazio **$\mathbb{R}^n$**).
  * **Se** $m = 1$, si ottiene un vettore riga.
  * **Se** $n = m = 1$, si tratta di uno scalare.
* **Memoria:** Informaticamente, una matrice viene salvata come un array. Memorizzare una matrice **$m \times n$** in formato *double* richiede uno spazio in memoria pari a **$8nm$** byte.

## 2. Tipologie di Matrici

**Conoscere la struttura di una matrice permette di ottimizzare l'occupazione di memoria, evitando di salvare gli elementi nulli**.

* **Trasposta ($A^T$):** Matrice in **$\mathbb{R}^{n \times m}$** ottenuta scambiando le righe con le colonne.
* **Simmetrica:** Matrice quadrata dove **$A = A^T$** (ovvero **$a_{ij} = a_{ji}$**). **I suoi autovalori sono tutti reali**.
* **Identità ($I$ o $I_n$):** Matrice quadrata con elementi uguali a **$1$** sulla diagonale principale e **$0$** altrove.
* **Diagonale ($D$):** Matrice quadrata dove **$d_{ij} = 0$** per **$i \neq j$**. **Ha al massimo **$n$** elementi non nulli**.
* **Triangolare Superiore ($U$):** Matrice quadrata in cui gli elementi sotto la diagonale sono nulli (**$u_{ij} = 0$** per **$i > j$**).
* **Triangolare Inferiore ($L$):** Matrice quadrata in cui gli elementi sopra la diagonale sono nulli (**$l_{ij} = 0$** per **$i < j$**).
* *Nota sulla memoria per le triangolari:* Gli elementi non nulli da memorizzare sono circa **$\frac{n^2}{2}$**.

## 3. Operazioni Matriciali e Complessità

| **Operazione**                 | **Definizione**                             | **Complessità Computazionale**                    |
| ------------------------------------ | ------------------------------------------------- | -------------------------------------------------------- |
| **Somma**                      | **$c_{ij} = a_{ij} + b_{ij}$**            | **$nm$** somme                                   |
| **Prodotto per scalare**       | **$c_{ij} = \lambda a_{ij}$**             | **$nm$** prodotti                                |
| **Prodotto Matrice-Matrice**   | **$c_{ij} = \sum_{k=1}^n a_{ik} b_{kj}$** | **$nmp$** (oppure **$n^3$** se quadrata) |
| **Prodotto Matrice-Vettore**   | **$y_i = \sum_{k=1}^n a_{ik} x_k$**       | **$nm$** (oppure **$n^2$** se quadrata)  |
| **Prodotto Scalare (vettori)** | **$s = u^T v = \sum_{k=1}^n u_k v_k$**    | **$n$** prodotti e **$n$** somme         |

**Proprietà del Prodotto Matriciale:**

* **Valgono la proprietà associativa e la distributiva (rispetto alla somma)**.
* **NON vale la proprietà commutativa**.
* **L'elemento neutro è la matrice Identità: $A I_n = I_m A$**.
* **Regola della trasposta: $(AB)^T = B^T A^T$**.
* *Nota Software:* Le operazioni matriciali sono solitamente implementate tramite librerie altamente ottimizzate chiamate **BLAS** (Basic Linear Algebra Subprograms).

## 4. Analisi Avanzata: Inversa, Determinante e Autovalori

### Inversa e Determinante

* **Inversa ($A^{-1}$):** Una matrice quadrata è invertibile (non singolare) se esiste una matrice **$X$** tale che **$AX = XA = I$**.
  * **Proprietà:** $(A^{-1})^{-1} = A$; **$(A^T)^{-1} = A^{-T}$**; **$(AB)^{-1} = B^{-1}A^{-1}$**.
* **Determinante ($\det(A)$):**
  * **Si calcola ricorsivamente con la Regola di Laplace**.
  * *Complessità:* Il calcolo computazionale tramite definizione ha un costo enorme, dell'ordine di **$n!$**.
  * **Teorema chiave:** Una matrice **$A$** è non singolare (invertibile) se e solo se **$\det(A) \neq 0$**.

### Autovalori e Autovettori

* **Definizione:** Un vettore non nullo **$x$** e uno scalare **$\lambda$** che soddisfano **$Ax = \lambda x$**.
* **Si trovano risolvendo l'equazione caratteristica:** **$\det(A - \lambda I) = 0$**.
* **Una matrice è non singolare se e solo se tutti i suoi autovalori sono diversi da zero**.
* **Raggio Spettrale ($\rho(A)$):** È il modulo massimo tra tutti gli autovalori di **$A$**, ovvero **$\rho(A) = \max_{i=1,\ldots,n} |\lambda_i|$**.

## 5. Norme (Vettoriali e Matriciali)

**Le norme sono fondamentali per esprimere la "misura" di vettori e matrici (equivalente al valore assoluto per gli scalari) e per quantificare la distanza tra essi, essenziale per il calcolo dell'errore relativo $\frac{\|x-y\|}{\|x\|}$**.

**Norme Vettoriali Principali:**

* **Norma 1:** $\|x\|_1 = \sum_{i=1}^n |x_i|$.
* **Norma 2 (Euclidea):** $\|x\|_2 = \sqrt{\sum_{i=1}^n x_i^2}$.
* **Norma $\infty$:** $\|x\|_\infty = \max_{i=1,\ldots,n} |x_i|$.
* *Nota:* Tutte le norme vettoriali sono equivalenti tra loro a meno di costanti.

**Norme Matriciali Indotte** $\|A\| = \sup_{x \neq 0} \frac{\|Ax\|}{\|x\|}$:

* **Norma 1:** Massimo della somma dei valori assoluti sulle *colonne*.
* **Norma 2:** Radice quadrata del raggio spettrale di **$A^T A$**, ovvero **$\sqrt{\rho(A^T A)}$**.
* **Norma $\infty$:** Massimo della somma dei valori assoluti sulle *righe*.
