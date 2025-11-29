# Algoritmi Avanzati

**Data lezione**: 18.09.2025

**Autori**: Pasqui

[back](appuntisupsimv/primo_semestre/algoritmi_avanzati/index.md)

# Stable Matching

Tenere presente il concetto di stabilità / instabilità e il perfect matching

#### Man Optimality slide 20

Tra tutti i casi stabili (si, esiste più casi, non uno solo).

L'uomo che si chiama Yancey (Y) cerca una donna.

Supponiamo che con l'algoritmo GS esce uno dei casi stabili dove Y sta con una che si chiama C => GS non è ottimale per Yancey.
Però la stabilità più ottimale (S) per Y starebbe con A e A preferirebbe stare con Y. 

=> Y \[..., A, ..., C, ...\]

Ma S esiste davvero?

Supponiamo che nel S, la ragazza B ha rigettato Y e sta con Z.
Quindi: Z => \[..., B, ..., A, ...\] oppure \[..., A, ..., B, ...\]. Ci sono due casi possibili in questo caso ma uno dei due non è possibile, ossia \[..., B, ..., A, ...\] perché se Y deve essere rigettato da A (ipotesi proposto all'inizio) perché è arrivato Zeus, non c'è nessuna ragione per cui B dev'essere prima di A nella lista di Z e la lista  \[..., A, ..., B, ...\] è sempre vera (non c'è un caso dove non è possibile).

=> A \[Z, ..., Y\] e Z \[..., A, ..., B, ...\]
e supponiamo che S sia stabile