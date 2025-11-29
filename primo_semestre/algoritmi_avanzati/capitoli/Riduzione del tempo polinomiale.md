
Dati il problema $A$ e il problema $B$. Possiamo esprimere la seguente notazione:

$$
A \leq_{p} B
$$
Se una volta risolto il problema $B$ e quindi ottiene lo stesso risultato di $A$, allora $A$ si riduce ($\leq_{p}$) in $B$. Se $A \leq_{p} B$ e $B$ può essere risolto in tempo polinomiale, allora anche $A$ può essere risolto in tempo polinomiale. Si può leggere anche $B$ è più "difficile" di $A$ perché $B$ comprende anche tutto ciò che è in $A$,

Facciamo un esempio pratico. $A$ è 3-SAT e $B$ è Independent Set, con la relazione $A \leq_{p} B$ significa che se risolvo il problema $B$ allora anche il problema $A$ è risolta. Quindi se ho un problema di 3-SAT, posso "trasformarlo" in Independent set, risolvo il problema e ottengo lo "stesso" risultato. 