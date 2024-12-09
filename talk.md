## Slide 2/22

Fai una piccola overview sostanzialmente leggendo l'indice

## Slide 3/22

Il problema della LCS sappiamo che può essere risolto in tempo polinomiale in O(n^m) con la programmazione dinamica per un numero di stringhe $m \geq 2$, con $n$ lunghezza massima delle stringhe. Quando non ci sono restrizioni su $m$ sappiamo che il problema è NP-completo.

Quello a cui siamo interessati è trovare un diverse set of solution per la longest common sequence di un set $S$ di stringhe, il tutto sotto la Hamming distance. La Hamming Distance tra due stringhe non è altro che il numero di caratteri diversi tra le due stringhe.

Guardiamo ad esempio questo caso

## Slide 4/22

Leggi l'esempio

## Slide 5/22

Cerchiamo di dare un po' di formalismo al problema. Considereremo due misure di diversità per un insieme di soluzioni che ammette ripetizioni:

- La prima è la max sum diversity
  $$D_{d_H}^{\text{sum}} (\mathcal{X}) &= \sum_{1 \leq i < j \leq K} d_H(X_i, X_j) & \text{\textsc{Max-Sum Diversity}}$$

Che sostanzialmente è la somma delle distanze di Hamming tra tutte le coppie di soluzioni.

La seconda è la max min diversity
$$D_{d_H}^{\text{min}} (\mathcal{X}) &= \min_{i < j} d_H(X_i, X_j) & \text{\textsc{Max-Min Diversity}}$$

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

- Il secondo problema è il \textsc{Diverse String Set}, che chiede se esiste un insieme di $K$ stringhe in un $\Sigma$-DAG con una distanza di Hamming maggiore o uguale a $\Delta$.

## Slide 7/22

Vediamo un attimo cosa sono questi \(\Sigma\)-DAGs.

Partiamo dalle definizioni fondamentali. Un alfabeto \(\Sigma\) è un insieme di simboli, mentre un linguaggio \(L\) è un insieme di stringhe costruite con questi simboli. Per descrivere un linguaggio, usiamo due misure principali: la lunghezza totale \(||L||\), che somma la lunghezza di tutte le stringhe in \(L\), e la lunghezza massima \(\text{maxLen}(L)\), che identifica la stringa più lunga. Una stringa di lunghezza fissata \(r\) è chiamata \(r\)-string.

Un \(\Sigma\)-DAG è un grafo diretto \(G = (V, E, s, t)\), dove i nodi \(V\) sono collegati da archi etichettati \(E = (v, c, w)\), con \(c \in \Sigma\). Questo grafo ha una sorgente \(s\) e un pozzo \(t\), tali che esiste almeno un cammino da \(s\) a ogni nodo. La dimensione del grafo, indicata con \(\text{size}(G)\), è data dal numero di archi etichettati.

## Slide 8/22

Passando alla rappresentazione del linguaggio, ogni cammino nel \(\Sigma\)-DAG, che collega \(s\) a \(t\), rappresenta una stringa generata concatenando le etichette degli archi del cammino. In questo senso, un \(\Sigma\)-DAG può rappresentare \(L(G)\), ovvero tutte le stringhe generate da \(s\) a \(t\). È interessante notare che un \(\Sigma\)-DAG è equivalente a un automa a stati finiti senza transizioni \(\epsilon\).

Una proprietà fondamentale è che, per qualsiasi insieme di stringhe \(L\), esiste un \(\Sigma\)-DAG \(G\) tale che il linguaggio \(L(G)\) sia esattamente \(L\), e la dimensione del grafo non superi \(||L||\). Inoltre, il grafo può essere costruito in modo efficiente, con un costo computazionale di \(O(||L|| \log |\Sigma|)\). Un'osservazione importante è che, se \(L\) contiene solo stringhe di lunghezza fissata \(r\), allora tutti i cammini da \(s\) a un nodo intermedio nel grafo hanno la stessa lunghezza, al massimo \(r\).

In sintesi, i \(\Sigma\)-DAGs rappresentano una soluzione elegante e ottimale per modellare linguaggi, offrendo una rappresentazione compatta e una costruzione efficiente. Questa combinazione li rende ideali per applicazioni che richiedono sia alte prestazioni che un uso ridotto della memoria."

## Slide 9/22

Un importante risultato sui \(\Sigma\)-DAGs è la loro applicazione al calcolo della LCS, la _Longest Common Subsequence_, per un insieme di stringhe. Il lemma 2.1 afferma che, dato un insieme \(S = \{S_1, \dots, S_m\}\) di \(m\) stringhe definite su un alfabeto \(\Sigma\), è possibile costruire un \(\Sigma\)-DAG \(G\) di dimensione polinomiale rispetto alla lunghezza massima delle stringhe in \(S\), indicata come \(\ell = \text{maxlen}(S)\). Questo \(\Sigma\)-DAG rappresenta esattamente il linguaggio delle LCS di \(S\) e può essere calcolato in tempo polinomiale.

La costruzione del \(\Sigma\)-DAG si basa su un grafo a griglia \(N\) \(m\)-dimensionale, dove ogni cammino \((s, t)\) rappresenta una sottosequenza comune delle stringhe in \(S\). Applicando una rimozione delle \(\varepsilon\), si ottiene il \(\Sigma\)-DAG finale con un numero di archi proporzionale a \(|\Sigma| \cdot \ell^m\), come dettagliato nella dimostrazione.

Un risultato importante derivato dal lemma è che se un problema di \textsc{Max-Min} o \textsc{Max-Sum Diverse String Set} è risolvibile in un tempo \(f(M, K, r, \Delta)\), allora lo stesso problema applicato alle LCS di stringhe può essere risolto in un tempo \(O(|\Sigma| \cdot \ell^m + f(\ell^m, K, r, \Delta))\). Questo dimostra un legame diretto tra problemi di diversità delle stringhe e l'utilizzo dei \(\Sigma\)-DAGs per rappresentare efficientemente le LCS.

## Slide 10/22

Adesso andremo a vedere che sia la versione Max Min che Max Sum del problema Diverse Set possono essere risolti in tempo e spazio polinomiale utilizzando la programmazione dinamica. Queste complessità vedremo come sono proporzionali alle dimensioni del SIGMA-DAG in input e degli interi \(K, r, \Delta\). Di conseguenza, dal lemma di prima, potremo concludere che anche i problemi di diversità delle LCS possono essere risolti in tempo polinomiale.

## Slide 11/22

Iniziamo con problema “Max-Min Diverse String Set”, facendo vedere una soluzione basata su un algoritmo di programmazione dinamica che ottimizza la selezione di insiemi di stringhe all’interno di un $Σ$-DAG $G$.

### Il Problema e la Sfida Computazionale

Il problema consiste nel determinare un insieme di $K$ cammini $(s,t)$ in un $Σ$-DAG $G = (V, E, s, t)$, dove ogni cammino è etichettato con una stringa di lunghezza $r$. L'obiettivo è massimizzare la diversità tra i cammini selezionati, misurata tramite le distanze di Hamming. Un approccio ingenuo potrebbe enumerare tutte le combinazioni di $K$ cammini per identificare il sottoinsieme ottimale. Tuttavia, poiché il numero totale di cammini $|L(G)|$ è esponenziale nel numero di archi di $G$, tale metodo è impraticabile anche per valori piccoli di $K$.

### L’Algoritmo di Programmazione Dinamica

L'algoritmo proposto sfrutta un modello di programmazione dinamica basato sul concetto di “pattern” per ridurre significativamente lo spazio di ricerca.

#### Tabella DP e Concetto di Pattern

L’idea centrale è rappresentare i cammini tramite una tabella DP ($\WT$) che tiene traccia delle “pattern”, ovvero combinazioni di stati e matrici di pesi:

- **Stato ($\vec{w}$)**: un $K$-tupla di nodi in $G$, ciascuno rappresentante la destinazione corrente di un cammino.
- **Matrice dei pesi ($Z$)**: una matrice triangolare superiore che registra le distanze di Hamming, troncate al valore massimo $Δ$, tra i cammini corrispondenti.

Ad esempio, per ogni $K$-tupla di cammini di lunghezza $d$ che terminano nei nodi specificati in $\vec{w}$, il pattern associato è $(\vec{w}, Z)$, dove $Z$ cattura le distanze tra i cammini.

#### Definizione della Tabella DP

Formalmente, $\WT(\vec{w}, Z)$ è una tabella booleana che indica se esiste almeno un $K$-tupla di cammini che conduce ai nodi specificati in $\vec{w}$ con una matrice dei pesi pari a $Z$. La costruzione della tabella avviene in maniera incrementale, calcolando per ogni nodo e lunghezza $d$ i possibili pattern derivanti da estensioni dei cammini precedenti.

## Slide 12/22

### Vantaggi dell’Approccio

L’utilizzo della tabella DP consente di ridurre drasticamente il numero di combinazioni da analizzare. La dimensione massima di $\WT$ è proporzionale a $|V|^K \cdot \Gamma$, dove $\Gamma = O(\Delta^{K^2}K^2)$ rappresenta il numero massimo di configurazioni della matrice $Z$. Per valori costanti di $K$, questo approccio è polinomiale rispetto alla dimensione del grafo $M$ e al parametro $\Delta$, garantendo un accesso rapido ai dati grazie a strutture come gli alberi bilanciati.

### Conclusioni

Questo algoritmo rappresenta un passo avanti rispetto ai metodi bruteforce, sfruttando una combinazione di strutture dati efficienti e un modello di programmazione dinamica avanzato. L’introduzione del concetto di pattern consente non solo di ridurre lo spazio computazionale necessario, ma anche di gestire in modo efficace problemi di diversità su istanze reali di grandi dimensioni.

## Slide 13/22

Vediamo un esempio di come funziona l'algoritmo

## Slide 14/22

Il problema \textsc{Max-Sum Diverse String Set} si occupa di trovare un insieme di stringhe diverse in termini di distanza di Hamming, massimizzando la somma delle distanze tra ogni coppia di stringhe. Per affrontare questo problema, ci basta modificare l'algoritmo precedente, introducendo una nuova tabella DP $\WT'$.

L'idea principale è che, anziché mantenere l'intera matrice dei pesi $(K \times K)$, è sufficiente calcolare e conservare solo la somma delle distanze di Hamming tra le stringhe. Per fare questo, si introduce una nuova tabella, denotata come $\WT'$, che tiene traccia delle soluzioni possibili per una sequenza di prefissi e la loro somma di distanze di Hamming.

## Slide 15/22

La ricorrenza utilizzata dall'algoritmo è la seguente: abbiamo prima un caso base in cui si parte dalla radice e una serie di casi di induzione che costruiscono progressivamente le soluzioni più lunghe. La condizione di aggiornamento richiede che esista un vettore di genitori per ciascuna stringa nella sequenza, con la somma delle distanze di Hamming che soddisfa un certo valore $z$.

L'algoritmo modificato ha una complessità polinomiale $O(\Delta K^2 M^K(\log |V| + \log \Delta))$, $M = \size(G)$ è il numero di archi. Questo lo rende significativamente più veloce rispetto alla versione \textsc{Max-Min} della variante del problema, che ha un costo computazionale maggiore per il fattore $O(\Delta^{K-1})$.

## Slide 16/22

Vediamo adesso cosa succede se non abbiamo alcun bound per K nel caso del problema Max Sum Diverse String Set. Per risolvere questo problema introduciamo il concetto di Local Search, che ci permette di calcolare soluzioni in spazi metrici finiti. Vedremo ppoi come applicare questo concetto al nostro problema nello spazio delle stringhe di equa lunghezza con distanza di Hamming come metrica.

## Slide 17/22

La procedura di ricerca locale \textsc{LocalSearch} è un algoritmo progettato per affrontare il problema della _Max-Sum Diversification_ in uno spazio metrico finito \(\sig D\). L'obiettivo è ottenere un sottoinsieme di \(K\) punti che massimizzi la diversità, calcolata come la somma massima delle distanze tra tutte le coppie di punti nel sottoinsieme selezionato, sotto una metrica semantica \(d\). Un aspetto chiave di questa procedura è la sottoprocedura _Farthest Point_, che trova il punto più distante da un insieme di punti già selezionato.

Un risultato importante dimostra che, se la metrica \(d\) è di _tipo negativo_ (una metrica con particolari proprietà di distanza) e il problema _Farthest Point_ può essere risolto in tempo polinomiale, l'algoritmo \textsc{LocalSearch} garantisce un'approssimazione con un rapporto di \((1 - \frac{2}{K})\) per qualsiasi valore di \(K \ge 1\). Ciò implica che la soluzione trovata è almeno una frazione \((1 - \frac{2}{K})\) della soluzione ottimale.

Il funzionamento dell'algoritmo prevede l'inizializzazione di un insieme arbitrario di \(K\) soluzioni, seguito da un ciclo iterativo in cui la soluzione è migliorata progressivamente, sostituendo uno degli elementi con un nuovo punto scelto in modo da massimizzare la somma delle distanze dal resto dell'insieme. Questo processo continua fino a raggiungere un numero specificato di iterazioni, e al termine, l'algoritmo restituisce l'insieme \(\X\) ottimizzato per la diversità.

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

Il **lemma** presentato dimostra che, per qualsiasi valore di $K \ge 1$ e $\Delta \ge 0$, dato un grafo $G$ e un sottoinsieme $\X' \subseteq L(G)$, l'algoritmo 3 è in grado di calcolare la stringa $r$-farthest $Y \in L(G)$ che massimizza la funzione di diversificazione $\Div[sum]{d_H}(\X', Y)$ in tempo e spazio $O(K \Delta M)$, dove $M$ è il numero di archi di $G$.

Infine gli autori fanno vedere che se K è parte dell'input, allora esiste un approccio di approssimazione polinomiale per il problema \textsc{Max-Sum Diverse String Set} su $Σ$-DAGs. Cosa vuol dire? Che per valori di K piccoli rispetto a un valore $\epsilon$ positivo, l'algoritmo esatto basato su \cref{alg:ExactMaxSumFarthest} risolve il problema in tempo polinomiale. Per valori di K più grandi, viene utilizzato un algoritmo di approssimazione che, combinando la ricerca locale e l'algoritmo di \cref{alg:ExactMaxSumFarthest}, raggiunge un fattore di approssimazione $1-\epsilon$ grazie alla proprietà di tipo negativo della metrica Hamming

## Slide 20/22

In questa sezione, vengono presentati algoritmi di tipo _fixed-parameter tractable_ (FPT) per i problemi _Max-Min_ e _Max-Sum Diverse String Set_, parametrizzati in funzione di $K$ e $r$.

## Slide 21/22

Un problema è considerato FPT se esiste un algoritmo che, dato un input $x$, può essere risolto in un tempo $f(\kappa(x)) \cdot |x|^c$, dove $f(\kappa)$ è una funzione computabile e $c$ è una costante positiva.

Per affrontare questi problemi, si combina la tecnica di _color-coding_ proposta da Alon, Yuster e Zwick con gli algoritmi di programmazione dinamica. L'idea principale consiste nell'assegnare casualmente colori agli archi di un grafo orientato aciclico ($\Sigma$-DAG), creando un grafo colorato noto come $C$-DAG. Viene dimostrato che è possibile ridurre un $C$-DAG a una versione più compatta, mantenendo invariato il linguaggio generato e con un numero di archi limitato da $(rK)^r$.

## Slide 22/22

Come vediamo dall'immagine, l'algoritmo prevede una fase di pre-elaborazione in cui il grafo $C$-DAG viene ridotto e una fase di ricerca per trovare un sottinsieme $\Delta$-diverso di dimensione $K$. Questa fase richiede un tempo complessivo di $O(2^{rK} r\log(rK) \cdot \size(G))$, dove $\size(G)$ rappresenta la dimensione dell'input. La probabilità di ottenere un risultato valido è elevata, e la ripetizione di questo processo consente di ottenere un algoritmo FPT.

Un risultato analogo è ottenuto anche per il problema _Max-Sum Diversity_, con una leggera modifica nei dettagli della fase di ricerca, che, in questo caso, ha una complessità di $O(\Delta K^2 M^K)$, dove $M$ è la dimensione dell'input.

## Slide 23/22

Affrontiamo i risultati di complessità relativi ai problemi di set di stringhe diversificate. Presentiamo risultati negativi che dimostrano che, quando $K$ è parte dell'input, i problemi \textsc{Max-Min} e \textsc{Max-Sum} sono NP-hard per qualsiasi valore costante di $r \ge 3$. Questo implica che, anche con un valore costante della lunghezza delle stringhe, non esiste un algoritmo polinomiale in grado di risolvere questi problemi in modo efficiente, a meno di dimostrare la P = NP. Inoltre, quando parametrizziamo questi problemi con $K$, risulta che sono W[1]-hard, anche quando la rappresentazione dell'input è un $\Sigma$-DAG.

Un ulteriore risultato interessante è che i problemi di \textsc{Diverse String Set} possono essere ridotti in tempo polinomiale a problemi di \textsc{Diverse LCSs} per due stringhe. Questa riduzione mantiene le caratteristiche di complessità dei problemi originali, mostrando che anche per le sequenze di lunghezza due, la difficoltà di calcolo rimane elevata.

Infine, deriviamo delle corollari fondamentali: la NP-hardness e la W[1]-hardness dei problemi di \textsc{Diverse String Set} si estendono ai problemi di \textsc{Diverse LCSs} per due stringhe di lunghezza $r$. Questi risultati confermano che trovare soluzioni efficienti a questi problemi è particolarmente complesso e pone delle sfide significative, sia in contesti classici che parametrici.
