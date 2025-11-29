
"Polinomiale nell'input" la parola l'input si intende la dimensione.
La funzione polinomiale si può esprimere $f(n)$ dove $n$ rappresenta la dimensione dell'input.

Ovviamente, la polinomiale in $n$ ovviamente è espresso in $n^c$, $c \in \mathbb{Z}$ dove $c$ è la costante . $2^n$, $n^{\log (n)}$, $n!$... **non** sono polinomiali.

Lo slowdown delle complessità computazionali è molto meglio rispetto a quelli esponenziali. Ecco una dimostrazioene con due esempi: $(2n)^2$ e $(2^n)^2$. Il primo caso diventa $2^2 \cdot n^2$ e notiamo che lo slowdown é "solo" la costante $2^2$. Mentre $(2^n)^2 = 2^n \cdot 2^n$ e lo slowdown è $2^n$ che rallenta esponenzialmente e quindi è molto inefficiente se la dimensione dell'input fosse molto grande.
