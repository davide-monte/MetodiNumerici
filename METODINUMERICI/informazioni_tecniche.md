Per aprire e visualizzare correttamente i file markdown, aprire il documento desiderato e digitare '**Ctrl + Shift + V**'


È un'ottima cosa che tu stia iniziando a pianificare ora. Un esame da 9 CFU di Metodi Numerici è denso, e il fatto che sia orale cambia le regole del gioco: non basta saper fare i calcoli, devi saper *spiegare* i concetti, dimostrare i teoremi principali e giustificare le scelte algoritmiche (condizionamento, stabilità, complessità computazionale).

Attualmente hai circa 33 giorni a disposizione. Le slide sono tante (presumibilmente arriveranno a 450-500 a fine corso) e la schematizzazione porta via molto tempo. Devi ottimizzare il tuo metodo per non ritrovarti a fine maggio con gli schemi finiti ma senza aver ripetuto ad alta voce.

Ecco un piano di studio strategico diviso per fasi, tenendo conto della difficoltà degli argomenti e del tuo metodo.

---

### 🗓️ Fase 1: Recupero e Schematizzazione Intensiva (07 Maggio - 22 Maggio)

L'obiettivo di questa fase è metterti in pari con le lezioni già spiegate, mantenendo una media di  **15-20 slide al giorno** .

* **07 - 08 Maggio | Chiusura Analisi degli Errori:**
  * Completa le lezioni dal 02/03 al 04/03.
  * *Focus Orale:* Assicurati di saper definire bene e distinguere l'errore inerente dall'errore algoritmico, e padroneggia il concetto di stabilità e condizionamento.
* **09 - 14 Maggio | Sistemi Lineari (Metodi Diretti):**
  * Lezioni dal 09/03 al 24/03 (Gauss, Cholesky, QR). È uno scoglio grosso.
  * *Focus Orale:* Impara a memoria e sappi spiegare l'ordine di complessità computazionale (es. **$O(n^3)$** per Gauss), il perché si usa il pivoting (evitare la divisione per numeri vicini allo zero e limitare l'amplificazione degli errori), e le differenze strutturali tra le fattorizzazioni. Ripassa bene le norme matriciali.
* **15 - 17 Maggio | Sistemi Lineari (Metodi Iterativi):**
  * Lezioni dal 25/03 al 31/03 (Jacobi, Gauss-Seidel).
  * *Focus Orale:* Condizioni sufficienti e necessarie per la convergenza (raggio spettrale **$\rho(B) < 1$**, matrici a dominanza diagonale).
* **18 - 22 Maggio | Equazioni Non Lineari:**
  * Lezioni dal 01/04 al 15/04 (Bisezione, Newton, Secanti).
  * *Focus Orale:* Ordine di convergenza dei metodi. Devi saper scrivere e spiegare la formula di Newton **$x_{k+1} = x_k - \frac{f(x_k)}{f'(x_k)}$** e interpretarla geometricamente.

---

### 🗓️ Fase 2: Mantenimento e Completamento (23 Maggio - 31 Maggio)

In questo periodo le lezioni finiranno. Il tuo scopo è seguire le ultime lezioni e schematizzare "in tempo reale".

* **23 - 27 Maggio | Interpolazione (Parte 1 e 2):**
  * Lezioni dal 20/04 al 05/05 (Polinomiale, Spline, 2D). Questo è l'argomento teoricamente più insidioso.
  * *Focus Orale:* Il Fenomeno di Runge è una tipica domanda d'esame. Devi saper spiegare perché aumentare il grado del polinomio con nodi equispaziati è una cattiva idea e come i nodi di Chebyshev risolvono il problema. Studia bene le proprietà delle Spline cubiche.
* **28 - 31 Maggio | Minimi Quadrati e Argomenti Finali:**
  * Lezioni dal 06/05 in poi.
  * *Focus Orale:* Differenza concettuale tra interpolazione (passare *esattamente* per i punti) e approssimazione ai minimi quadrati (minimizzare la norma del residuo).
  * Usa gli ultimi due giorni di maggio come "giorni cuscinetto" per recuperare eventuali ritardi.

---

### 🗓️ Fase 3: Ripasso Attivo e Simulazione Orale (01 Giugno - 08 Giugno)

Ora devi posare la penna. Gli schemi sono finiti. Da qui in poi si  **parla** .

* **01 - 02 Giugno:** Ripasso Errori + Sistemi Lineari.
* **03 - 04 Giugno:** Ripasso Eq. Non Lineari + Interpolazione.
* **05 Giugno:** Ripasso Minimi Quadrati + Ultimi argomenti.
* **06 - 08 Giugno:** Simulazioni complete. Estrai argomenti a caso e prova a spiegarli per 10-15 minuti ciascuno, possibilmente davanti a uno specchio o registrandoti. Scrivi le formule e i teoremi su un foglio bianco mentre parli, esattamente come faresti davanti al professore.

---

### 💡 Consigli per l'Esame Orale di Metodi Numerici

1. **Non trascurare i Colab/Python:** Il professore ha inserito un sacco di notebook. Anche se è un orale, potrebbe chiederti: *"Come implementeresti questo in Python?"* o  *"Che struttura dati useresti?"* . Non devi sapere il codice a memoria riga per riga, ma devi conoscere la  **logica dell'algoritmo** .
2. **Attenzione al "Tutto Schematizzato":** Schematizzare 400 slide rischia di farti perdere troppo tempo. Cerca di fare schemi  *agili* : scrivi solo gli enunciati dei teoremi, la formula chiave e la complessità. Il resto del discorso costruiscilo a voce.
3. **Il filo conduttore:** Metodi Numerici si basa su un trade-off continuo tra **precisione** (errore), **costo computazionale** (numero di operazioni) e  **stabilità** . Ogni volta che studi un algoritmo nuovo, chiediti: *Quanto costa? Quanto è preciso? Sotto quali ipotesi fallisce?*
