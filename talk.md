## Slide 2/22

Fai una piccola overview sostanzialmente leggendo l'indice

## Slide 3/22

Il problema della LCS sappiamo che può essere risolto in tempo polinomiale in O(n^m) con la programmazione dinamica per un numero di stringhe $m \geq 2$, con $n$ lunghezza massima delle stringhe. Quando non ci sono restrizioni su $m$ sappiamo che il problema è NP-completo.

Quello a cui siamo interessati è trovare un diverse set of solution per la longest common sequence di un set $S$ di stringhe, il tutto sotto la Hamming distance. La Hamming Distance tra due stringhe non è altro che il numero di caratteri diversi tra le due stringhe. Dove per diverse set of solution intendiamo un insieme di soluzioni che massimizzano la diversità tra le soluzioni stesse.

Guardiamo ad esempio questo caso

## Slide 5/22

Cerchiamo di dare un po' di formalismo al problema. Considereremo due misure di diversità per un insieme di soluzioni che ammette ripetizioni:

- La prima è la max sum diversity

  $$D_{d_H}^{\text{sum}} (\mathcal{X}) = \sum_{1 \leq i < j \leq K} d_H(X_i, X_j) \quad \text{Max-Sum Diversity}$$

Che sostanzialmente è la somma delle distanze di Hamming tra tutte le coppie di soluzioni.

La seconda è la max min diversity

$$D_{d_H}^{\text{min}} (\mathcal{X}) = \min_{i < j} d_H(X_i, X_j) \quad \text{Max-Min Diversity}$$

Che è la distanza minima tra tutte le coppie di soluzioni.

## Slide 6/22

Ci concentreremo a risolvere due problemi

<!-- \begin{alertblock}{Problem 1: \textsc{Diverse LCSs with Diversity measure} $D_{d_H}^{\tau}$}
    \emph{Input:} A set $S = \{S_1, S_2, \ldots, S_m\}$ of $m \geq 2$ strings over $\Sigma$, an integer $K \geq 1$ and $\Delta \geq 0$. \\
    \emph{Question:} Is there some set $\mathcal{X} \subseteq LCS(S)$ such that $\mathcal{X}= K$ and $D_{d_H}^{\tau}(\mathcal{X}) \geq \Delta$?
\end{alertblock}
\begin{alertblock}{Problem 2: \textsc{Diverse String Set}}
    \emph{Input:} $K, r, \Delta \in \mathbb{Z}$ and a $\Sigma$-DAG $G$ for a set $L(G) \subseteq \Sigma^r$ of strings. \\
    \emph{Question:} Decide if there exists some subset $\mathcal{X} \subseteq L(G)$ such that $|\mathcal{X}| = K$ and $D_{d_H}^{\tau}(\mathcal{X}) \geq \Delta$.
\end{alertblock} -->

- Il primo problema è il \textsc{Diverse LCSs with Diversity measure} $D_{d_H}^{\tau}$, che chiede se esiste un insieme di $K$ LCS di $S$ con una distanza di Hamming maggiore o uguale a $\Delta$.

Andremo a vedere la complessità computazionale di questo prima problema da diversi punti di vista: sia da algoritmi di approssimazione, sia da via complessità parametrizzata. A questo scopo infatti lavoreremo in un setting più generale, chiamato "Diverse String Set", dove un set di string è rappresentato implicitamente da un DAG con archi etichettati, chiamato $\Sigma$-DAG. Questo storerà in maniera succinta una collezione di stringhe associate ai path che partono da una certa sorgente e arrivano ad una certa destinazione.

Consideriamo ora il secondo problema, dove $\tau$ può essere sia sum che min

- Il secondo problema è il Diverse String Set, che chiede se esiste un insieme di $K$ stringhe in un $\Sigma$-DAG con una distanza di Hamming maggiore o uguale a $\Delta$.

## Slide 7/22

Vediamo un attimo cosa sono questi $\Sigma$-DAGs.

Partiamo dalle definizioni fondamentali. Un alfabeto $\Sigma$ è un insieme di simboli, mentre un linguaggio $L$ è un insieme di stringhe costruite con questi simboli. Per descrivere un linguaggio, usiamo due misure principali: la lunghezza totale $||L||$, che somma la lunghezza di tutte le stringhe in $L$, e la lunghezza massima $\text{maxLen}(L)$, che identifica la stringa più lunga. Una stringa di lunghezza fissata $r$ è chiamata $r$-string.

Un $\Sigma$-DAG è un grafo diretto $G = (V, E, s, t)$, dove i nodi $V$ sono collegati da archi etichettati $E = (v, c, w)$, con $c \in \Sigma$. Questo grafo ha una sorgente $s$ e un pozzo $t$, tali che esiste almeno un cammino da $s$ a ogni nodo. La dimensione del grafo, indicata con $\text{size}(G)$, è data dal numero di archi etichettati.

## Slide 8/22

Passando alla rappresentazione del linguaggio, ogni cammino nel $\Sigma$-DAG, che collega $s$ a $t$, rappresenta una stringa generata concatenando le etichette degli archi del cammino. In questo senso, un $\Sigma$-DAG può rappresentare $L(G)$, ovvero tutte le stringhe generate da $s$ a $t$. È interessante notare che un $\Sigma$-DAG è equivalente a un automa a stati finiti senza transizioni $\epsilon$.

Una proprietà fondamentale è che, per qualsiasi insieme di stringhe $L$, esiste un $\Sigma$-DAG $G$ tale che il linguaggio $L(G)$ sia esattamente $L$, e la dimensione del grafo non superi $||L||$. Inoltre, il grafo può essere costruito in modo efficiente, con un costo computazionale di $O(||L|| \log |\Sigma|)$. Un'osservazione importante è che, se $L$ contiene solo stringhe di lunghezza fissata $r$, allora tutti i cammini da $s$ a un nodo intermedio nel grafo hanno la stessa lunghezza, al massimo $r$.

## Slide 9/22

Un importante risultato sui $\Sigma$-DAGs è la loro applicazione al calcolo della LCS, la _Longest Common Subsequence_, per un insieme di stringhe. Il lemma afferma che, dato un insieme $S = \{S_1, \dots, S_m\}$ di $m$ stringhe definite su un alfabeto $\Sigma$, è possibile costruire un $\Sigma$-DAG $G$ di dimensione polinomiale rispetto alla lunghezza massima delle stringhe in $S$ tale che rappresenti esattamente il linguaggio delle LCS di $S$ e può essere calcolato in tempo polinomiale.

La costruzione del $\Sigma$-DAG si basa su un grafo a griglia $N$ $m$-dimensionale, dove ogni cammino $(s, t)$ rappresenta una sottosequenza comune delle stringhe in $S$. Applicando una rimozione delle $\varepsilon$, si ottiene il $\Sigma$-DAG finale con un numero di archi proporzionale a $|\Sigma| \cdot \ell^m$, come dettagliato nella dimostrazione.

Un risultato importante derivato dal lemma è che se un problema di \textsc{Max-Min} o \textsc{Max-Sum Diverse String Set} è risolvibile in un tempo $f(M, K, r, \Delta)$, allora lo stesso problema applicato alle LCS di stringhe può essere risolto in un tempo $O(|\Sigma| \cdot \ell^m + f(\ell^m, K, r, \Delta))$. Questo dimostra un legame diretto tra problemi di diversità delle stringhe e l'utilizzo dei $\Sigma$-DAGs per rappresentare efficientemente le LCS.

## Slide 10/22

Adesso andremo a vedere che sia la versione Max Min che Max Sum del problema Diverse Set possono essere risolti in tempo e spazio polinomiale utilizzando la programmazione dinamica. Queste complessità vedremo come sono proporzionali alle dimensioni del SIGMA-DAG in input e degli interi $K, r, \Delta$. Di conseguenza, dal lemma di prima, potremo concludere che anche i problemi di diversità delle LCS possono essere risolti in tempo polinomiale.

## Slide 11-12/22

Il problema \textsc{Max-Min Diverse String Set} mira a selezionare un insieme di stringhe che massimizza la diversità minima tra le coppie di stringhe, utilizzando la distanza di Hamming. Inizialmente, un approccio brute force sarebbe teoricamente possibile: enumerare tutte le combinazioni di $K$ cammini $(s,t)$ in un grafo $G$ e poi scegliere una soluzione $\Delta$-diversa. Tuttavia, questa strategia è impraticabile perché la complessità cresce esponenzialmente con il numero di archi di $G$.

La soluzione proposta si basa invece sulla programmazione dinamica. La chiave è rappresentare ogni $K$-tuple di cammini di lunghezza $d$ con un pattern $(\mathbf{w}, \mathbf{Z})$. Qui, $\mathbf{w}$ identifica gli endpoint dei cammini, mentre $\mathbf{Z}$ è una matrice triangolare superiore che cattura le distanze di Hamming limitate da $\Delta$. Utilizziamo una tabella DP, denominata \texttt{Weights}, che associa ad ogni pattern un valore booleano, indicando se quel pattern è realizzabile con cammini di lunghezza $d$.

L'algoritmo segue una logica iterativa: per ogni profondità $d$, esplora tutte le possibili transizioni dei cammini e aggiorna \texttt{Weights} considerando le matrici delle distanze tra le stringhe. Alla fine, verifica se esiste una configurazione compatibile con il requisito di diversità minima $\Delta$. Nonostante la complessità, il metodo è drasticamente più efficiente rispetto al brute force, rendendo possibile la risoluzione di casi pratici su grafi rappresentati in modo compatto come $\Sigma$-DAG. La dimensione massima di $WT$ è proporzionale a $|V|^K \cdot \Gamma$, dove $\Gamma = O(\Delta^{K^2}K^2)$ rappresenta il numero massimo di configurazioni della matrice $Z$. Per valori costanti di $K$, questo approccio è polinomiale rispetto alla dimensione del grafo $M$ e al parametro $\Delta$, garantendo un accesso rapido ai dati grazie a strutture come gli alberi bilanciati.

## Slide 13/22

L'esempio mostrato nell'immagine illustra l'applicazione dell'algoritmo su un $\Sigma$-DAG $G_1$, costruito per rappresentare tutte le sottosequenze comuni massime (Longest Common Subsequences, LCS) di due stringhe $X_1 = ABABCDDEE$ e $Y_1 = ABCBAEEDD$, con $K = 3$.

### Passaggi principali del run dell'algoritmo:

#### Parte (a): Il $\Sigma$-DAG di input

1. **Definizione del grafo**: I nodi rappresentano posizioni nelle stringhe $X_1$ e $Y_1$, mentre gli archi corrispondono alle corrispondenze tra simboli.

   - Ad esempio, i nodi $s$ e $t$ rappresentano rispettivamente la partenza e la destinazione nel grafo.
   - I pesi sugli archi corrispondono ai caratteri $\Sigma = \{A, B, C, D, E\}$.

2. **Obiettivo**: Utilizzare il grafo $G_1$ per selezionare $K = 3$ cammini distinti, ciascuno corrispondente a una sottosequenza comune massima, garantendo che la diversità minima (misurata con la distanza di Hamming) sia massimizzata.

#### Parte (b): Run dell'algoritmo

1. **Inizializzazione**:

   - L'algoritmo costruisce una tabella dinamica (\texttt{Weights}) che tiene traccia dei pattern $(\mathbf{w}, \mathbf{Z})$.
   - Ogni pattern cattura $K = 3$ cammini attualmente considerati e la loro matrice di distanza di Hamming.

2. **Evoluzione delle configurazioni**:

   - Nodi iniziali come $aaa, bbb, sss$ rappresentano configurazioni di partenza per i cammini.
   - A ogni passo, l'algoritmo verifica transizioni valide dai nodi correnti, costruendo nuove configurazioni e aggiornando $\mathbf{Z}$.

3. **Massimizzazione della diversità**:
   - I nodi $ccc$ e $fff$ cerchiati in rosso rappresentano configurazioni che soddisfano la condizione di diversità minima $\Delta = 3$.
   - La matrice $\mathbf{Z}$ accanto al grafo mostra le distanze di Hamming tra coppie di cammini nei nodi finali ($ttt$).

### Risultato finale

Alla fine, l'algoritmo identifica una configurazione valida in cui i $K = 3$ cammini hanno la distanza minima richiesta $\Delta = 3$, rispettando i vincoli del problema. Questo dimostra l'efficacia della programmazione dinamica nell'individuare soluzioni ottimali in un grafo compatto come $G_1$.

## Slide 14/22

Possiamo risolvere questo problema facendo una modifica all'algoritmo precedente.

Adesso invece di salvare l'intera matrice $Z$ di dimensione $K \times K$, contenente tutte le distanze di Hamming tra le stringhe del nostro insieme, possiamo comprimere le informazioni rappresentando semplicemente la somma totale delle distanze di Hamming tra tutte le coppie di stringhe:

$z = \sum\_{i<j} d_H(\text{str}(P_i), \text{str}(P_j))$.

Questo approccio riduce significativamente la complessità del calcolo, rendendo più gestibile il problema dal punto di vista computazionale.

Passiamo quindi alla definizione della nuova tabella di programmazione dinamica (DP). Per un vettore $\mathbf{w} = (w_1, \dots, w_K)$, che rappresenta i nodi terminali di $K$-cammini in un $\Sigma$-DAG $G$, definiamo un nuovo stato $\texttt{Weights}(\mathbf{w}, z)$:

- $\texttt{Weights}(\mathbf{w}, z) = 1$ se e solo se esistono $K$-cammini prefisso di lunghezza $d$, terminanti rispettivamente nei nodi $w_1, \dots, w_K$, con somma delle distanze di Hamming $z$.

Questa rappresentazione consente di catturare la diversità tra i cammini in maniera compatta ed efficiente.

## Slide 15/22

L'algoritmo si sviluppa attraverso i seguenti passaggi:

1. **Inizializzazione**:
   Per ogni possibile valore di $ Z $, inizializziamo $\texttt{Weights}(\mathbf{s}, Z)$ a 0, tranne che per la configurazione iniziale in cui $ Z = 0 $, che corrisponde alla situazione di partenza con nessuna distanza accumulata.

2. **Iterazione sulla profondità**:
   Iteriamo dalla profondità $ d = 1 $ fino alla massima profondità $ r $ del $\Sigma$-DAG. A ogni livello, consideriamo tutti i possibili vettori $\mathbf{v} = (v_1, \dots, v_K)$, dove $v_1, \dots, v_K$ rappresentano i nodi attuali dei cammini.

3. **Aggiornamento delle configurazioni**:
   Per ciascun possibile arco in uscita da $v_1, \dots, v_K$, calcoliamo i nuovi nodi terminali $\mathbf{w} = (w_1, \dots, w_K)$ e aggiorniamo la tabella $\texttt{Weights}(\mathbf{w}, Z)$.
   Qui, il valore $ Z $ viene aggiornato considerando la distanza di Hamming accumulata, con l'aggiunta di 1 per ogni coppia di archi in cui i caratteri $ c_i $ e $ c_j $ differiscono.

4. **Condizione di arresto**:
   Una volta raggiunta la profondità massima $ r $, l'algoritmo verifica se esiste una configurazione valida in cui la somma delle distanze $ z $ soddisfa il vincolo di diversità minima $\Delta$. Se tale configurazione esiste, il risultato è _Yes_, altrimenti _No_.

L'algoritmo è estremamente efficiente rispetto alla dimensione del problema:

- Richiede tempo e spazio $ O(\Delta K^2 M^K (\log |V| + \log \Delta)) $, dove:
  - $\Delta$ è la diversità minima richiesta.
  - $K$ è il numero di cammini richiesti.
  - $M$ è la dimensione del $\Sigma$-DAG.
  - $|V|$ è il numero di nodi nel grafo.

Questa complessità dimostra che l'algoritmo è scalabile, anche per grafi di grandi dimensioni e valori elevati di $K$.

## Slide 16/22

Vediamo adesso cosa succede se non abbiamo alcun bound per K nel caso del problema Max Sum Diverse String Set. Per risolvere questo problema introduciamo il concetto di Local Search, che ci permette di calcolare soluzioni in spazi metrici finiti. Vedremo ppoi come applicare questo concetto al nostro problema nello spazio delle stringhe di equa lunghezza con distanza di Hamming come metrica.

## Slide 17/22

La procedura di ricerca locale \textsc{LocalSearch} è un algoritmo progettato per affrontare il problema della _Max-Sum Diversification_ in uno spazio metrico finito $D$. L'obiettivo è ottenere un sottoinsieme di $K$ punti che massimizzi la diversità, calcolata come la somma massima delle distanze tra tutte le coppie di punti nel sottoinsieme selezionato, sotto una metrica semantica $d$. Un aspetto chiave di questa procedura è la sottoprocedura _Farthest Point_, che trova il punto più distante da un insieme di punti già selezionato.

Un risultato importante dimostra che, se la metrica $d$ è di _tipo negativo_ (una metrica con particolari proprietà di distanza) e il problema _Farthest Point_ può essere risolto in tempo polinomiale, l'algoritmo \textsc{LocalSearch} garantisce un'approssimazione con un rapporto di $(1 - \frac{2}{K})$ per qualsiasi valore di $K \ge 1$. Ciò implica che la soluzione trovata è almeno una frazione $(1 - \frac{2}{K})$ della soluzione ottimale.

Il funzionamento dell'algoritmo prevede l'inizializzazione di un insieme arbitrario di $K$ soluzioni, seguito da un ciclo iterativo in cui la soluzione è migliorata progressivamente, sostituendo uno degli elementi con un nuovo punto scelto in modo da massimizzare la somma delle distanze dal resto dell'insieme. Questo processo continua fino a raggiungere un numero specificato di iterazioni, e al termine, l'algoritmo restituisce l'insieme $X$ ottimizzato per la diversità.

Ciò che resta da capire è come risolvere in maniera efficace il passaggio alla linea 4 dell'algoritmo, ovvero trovare Y

## Slide 18/22

Dato un insieme $X' \subseteq \Sigma^r$ di stringhe, vogliamo trovare la stringa Y più lontana da tutti gli elementi di $X'$, ovvero la stringa che massimizza la somma delle distanze di Hamming tra Y e gli elementi di $X'$. Questo problema è noto come _Farthest String_ e può essere risolto in tempo polinomiale.

Consideriamo questo algoritmo decisionale, che "decide" una tale stringa Y. Possiamo successivamente modificarlo per determinare esplicitamente questa Y.

<!--
  \begin{algorithm}[H] \label{alg:ExactMaxSumFarthest}
      \footnotesize{\caption{An exact algorithm for solving the \textsc{Max-Sum Farthest} $r$-\textsc{String} problem}}
      \footnotesize{\begin{algorithmic}[1]
          \State Set $Weights(s, z) := 0$ for all $z \in [\Delta]_+$, and $Weights(s, 0) := 1$\;
          \For{$d := 1, \dots, r$}
              \For{$0\le u\le \Delta$ such that $Weights(v, u) := 1$}
                  \State Set $Weights(w, z) := 1$ for $z := u + \sum_{i \in [K]} \mathbbm{1}\{ c \not= X_i[d] \}$ \Comment{Update}
              \EndFor
          \EndFor
          \State Answer YES if $Weights(t, \Delta) = 1$, and NO otherwise \Comment{Decide}
      \end{algorithmic}}
  \end{algorithm} -->

prende in input un $\Sigma$-DAG $G$ per un insieme $L(G) \subseteq \Sigma^r$ di stringhe, un set $X' = \{X_1, \dots, X_K\}$ di stringhe di lunghezza $r$, ed un intero $\Delta \ge 0$. L'algoritmo calcola se esiste una stringa $Y \in L(G)$ tale che la somma delle distanze di Hamming tra $Y$ e gli elementi di $X'$ sia almeno $\Delta$.

Questo algoritmo è ottenuto dall'algoritmo originale sostanzialmente fissando $K-1$ cammini e cercando solo un cammino rimanente che massimizzi la somma delle distanze di Hamming. La usa correttezza e complessità seguono direttamente dall'algoritmo originale.

## Slide 19/22

<!-- Il **lemma** presentato dimostra che, per qualsiasi valore di $K \ge 1$ e $\Delta \ge 0$, dato un grafo $G$ e un sottoinsieme $X' \subseteq L(G)$, l'algoritmo 3 è in grado di calcolare la stringa $r$-farthest $Y \in L(G)$ che massimizza la funzione di diversificazione $Div[sum]{d_H}(X', Y)$ in tempo e spazio $O(K \Delta M)$, dove $M$ è il numero di archi di $G$. -->

Infine gli autori fanno vedere che se K è parte dell'input, allora esiste un approccio di approssimazione polinomiale per il problema Max-Sum Diverse String Set su $Σ$-DAGs. Cosa vuol dire? Che per valori di K piccoli rispetto a un valore $\epsilon$ positivo, l'algoritmo esatto basato su \cref{alg:ExactMaxSumFarthest} risolve il problema in tempo polinomiale. Per valori di K più grandi, viene utilizzato un algoritmo di approssimazione che, combinando la ricerca locale e l'algoritmo di \cref{alg:ExactMaxSumFarthest}, raggiunge un fattore di approssimazione $1-\epsilon$ grazie alla proprietà di tipo negativo della metrica Hamming

## Slide 20/22

In questa sezione, vengono presentati algoritmi di tipo _fixed-parameter tractable_ (FPT) per i problemi _Max-Min_ e _Max-Sum Diverse String Set_, parametrizzati in funzione di $K$ e $r$.

## Slide 21/22

Un problema è considerato FPT se esiste un algoritmo che, dato un input $x$, può essere risolto in un tempo $f(\kappa(x)) \cdot |x|^c$, dove $f(\kappa)$ è una funzione computabile e $c$ è una costante positiva.

Per affrontare questi problemi, si combina la tecnica di _color-coding_ proposta da Alon, Yuster e Zwick con gli algoritmi di programmazione dinamica. L'idea principale consiste nell'assegnare casualmente colori agli archi di un grafo orientato aciclico ($\Sigma$-DAG), creando un grafo colorato noto come $C$-DAG. Viene dimostrato che è possibile ridurre un $C$-DAG a una versione più compatta, mantenendo invariato il linguaggio generato e con un numero di archi limitato da $(rK)^r$.

## Slide 22/22

Come vediamo dall'immagine, l'algoritmo prevede una fase di pre-elaborazione in cui il grafo $C$-DAG viene ridotto e una fase di ricerca per trovare un sottinsieme $\Delta$-diverso di dimensione $K$. Questa fase richiede un tempo complessivo di $O(2^{rK} r\log(rK) \cdot size(G))$, dove $size(G)$ rappresenta la dimensione dell'input. La probabilità di ottenere un risultato valido è elevata, e la ripetizione di questo processo consente di ottenere un algoritmo FPT.

Un risultato analogo è ottenuto anche per il problema _Max-Sum Diversity_, con una leggera modifica nei dettagli della fase di ricerca, che, in questo caso, ha una complessità di $O(\Delta K^2 M^K)$, dove $M$ è la dimensione dell'input.

## Slide 23/22

Affrontiamo i risultati di complessità relativi ai problemi di set di stringhe diversificate. Presentiamo risultati negativi che dimostrano che, quando $K$ è parte dell'input, i problemi \textsc{Max-Min} e \textsc{Max-Sum} sono NP-hard per qualsiasi valore costante di $r \ge 3$. Questo implica che, anche con un valore costante della lunghezza delle stringhe, non esiste un algoritmo polinomiale in grado di risolvere questi problemi in modo efficiente, a meno di dimostrare la P = NP. Inoltre, quando parametrizziamo questi problemi con $K$, risulta che sono W[1]-hard, anche quando la rappresentazione dell'input è un $\Sigma$-DAG. Dove per W[1]-hard intendiamo che non esiste un algoritmo FPT per questi problemi, a meno che non si dimostri che FPT = W[1], dove W[1] è una classe di complessità parametrizzata.

Un ulteriore risultato interessante è che i problemi di \textsc{Diverse String Set} possono essere ridotti in tempo polinomiale a problemi di \textsc{Diverse LCSs} per due stringhe. Questa riduzione mantiene le caratteristiche di complessità dei problemi originali, mostrando che anche per le sequenze di lunghezza due, la difficoltà di calcolo rimane elevata.

Infine, deriviamo delle corollari fondamentali: la NP-hardness e la W[1]-hardness dei problemi di \textsc{Diverse String Set} si estendono ai problemi di \textsc{Diverse LCSs} per due stringhe di lunghezza $r$. Questi risultati confermano che trovare soluzioni efficienti a questi problemi è particolarmente complesso e pone delle sfide significative, sia in contesti classici che parametrici.
