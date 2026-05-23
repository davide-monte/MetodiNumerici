# Richiami su NumPy: Fondamenti per Metodi Numerici

Libreria di riferimento: `import numpy as np`

## 1. Creazione e Attributi degli Array (1D e 2D)

### Creazione base
* **`np.array(lista_o_tupla)`**: Crea un array (1D o 2D se riceve liste annidate). Effettua l'upcasting automatico (es. se ci sono interi e float, tutto diventa `float64` per non perdere precisione).
* **`.dtype`**: Attributo che restituisce il tipo di dato degli elementi dell'array.
* **`.astype(tipo)`**: Metodo per forzare il recast dell'array a un tipo specifico (es. `float64`).

### Attributi dimensionali
* **`.ndim`**: Restituisce il numero di dimensioni dell'array (es. `1` per un vettore, `2` per una matrice).
* **`.shape`**: Restituisce una tupla con le dimensioni dell'array `(numero_righe, numero_colonne)`.
* **`.size`**: Restituisce il numero totale di elementi contenuti nell'array.
* **`.flags`**: Restituisce informazioni sui criteri di allocazione in memoria dell'array (es. contiguità in C o Fortran).

---

## 2. Array e Matrici Speciali

### Generazione di vettori
* **`np.empty(shape, dtype)`**: Alloca memoria per un array senza inizializzarne i valori (i dati inseriti sono casuali in base a ciò che c'era in memoria).
* **`np.arange(start, stop)`**: Crea un array di interi consecutivi da `start` a `stop - 1`.
* **`np.linspace(start, stop, num)`**: Genera un vettore di `num` elementi equispaziati tra `start` e `stop` (estremi inclusi). Utile per la discretizzazione di intervalli.
* **`np.random.randn(n)`**: Genera un vettore di `n` elementi estratti da una distribuzione normale standard.

### Generazione di matrici (2D)
* **`np.zeros((righe, colonne))`**: Crea una matrice di zeri.
* **`np.eye(righe, colonne)`**: Crea una matrice identità (1 sulla diagonale principale, 0 altrove).

### Gestione delle Diagonali
* **`np.diag(vettore)`**: Trasforma un vettore 1D in una matrice diagonale (elementi del vettore sulla diagonale principale).
* **`np.diagonal(matrice, k)`**: Estrae la $k$-esima diagonale da una matrice e la restituisce come vettore 1D.
* **`np.tril(matrice)`**: Estrae la parte **triangolare inferiore** (lower) di una matrice.
* **`np.triu(matrice)`**: Estrae la parte **triangolare superiore** (upper) di una matrice.

---

## 3. Indicizzazione, Slicing e Gestione della Memoria

### Accesso ed Estrazione
* **Indicizzazione singola**: `a[0]` (gli indici partono sempre da 0).
* **Slicing**: `a[start:stop]` (seleziona gli elementi da `start` a `stop - 1`). Nelle matrici si usa la virgola: `A[:, 0]` (tutta la prima colonna) o `A[0, :]` (tutta la prima riga).
* **Indicizzazione con liste/array**: `a[[0, 2, 4]]` seleziona elementi in posizioni specifiche.
* **Indicizzazione logica**: `a[[True, False, True]]` seleziona solo gli elementi corrispondenti a `True`.

### Memoria e Copie
* **Modifica in-place**: Modificando uno slice (es. `a[0:2] = 4`), si modifica l'array originale in memoria.
* **`.copy()`**: Esegue una *copia profonda* (deep copy). Se si modifica la copia, l'array originale rimane inalterato.

---

## 4. Operazioni Aritmetiche e Algebra Lineare

### Operazioni Element-wise (Componente per componente)
Queste operazioni si applicano parallelamente su tutti gli elementi dell'array.
* **Con scalari**: `a + 1`, `a ** 2`, `2 ** a`.
* **Tra array**: `a * b` (prodotto elemento per elemento), `a ** b`.
* **`.prod()` / `.sum()`**: Moltiplica o somma tutti gli elementi di un array (es. calcolo del fattoriale).

### Algebra Lineare
* **`np.dot(a, b)`**: Prodotto scalare tra due vettori 1D. Equivalente a `(a * b).sum()`.
* **`.T`**: Traspone una matrice (scambia righe con colonne).
* **`@` (Prodotto Matriciale)**: Esegue la moltiplicazione riga per colonna tra due matrici (`A @ B`) o tra una matrice e un vettore (`A @ x`).

---

## 5. Efficienza e Performance
NumPy implementa le operazioni in linguaggio C precompilato. Le operazioni vettorializzate di NumPy (come `np.dot` o `.sum()`) sono ordini di grandezza più veloci ed efficienti rispetto all'uso dei classici cicli `for` in Python puro. (Valutabile tramite la libreria `timeit`).