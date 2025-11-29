
1. Interval scheduling
2. Bipartite matching
3. Indipendent set


Perfetto 👍, da **slide 26 in poi** il corso si sposta dai _matching problems_ ad altri **problemi rappresentativi di progettazione algoritmica**.  
Vediamoli con calma e con un po’ di approfondimento tecnico:

---

### 🔹 Slide 26 – Interval Scheduling (Selezione Intervalli)

- **Input:** un insieme di lavori/intervalli, ognuno con tempo di inizio e fine.
- **Goal:** scegliere il massimo numero di lavori compatibili (nessuna sovrapposizione).
- **Esempio tipico:** prenotazione di aule, allocazione di risorse, pianificazione di riunioni.

**Algoritmo ottimo:**

- Ordinare i job secondo **tempo di fine crescente** e scegliere greedy quello che finisce prima, scartando i conflittuali.
- Complessità: O(nlogn) (per l’ordinamento).
- Risultato garantito: sempre massimo numero di intervalli.

---

### 🔹 Slide 28 – Weighted Interval Scheduling (Intervalli pesati)

- **Input:** come prima, ma ogni intervallo ha un **peso/valore**.
- **Goal:** scegliere un insieme compatibile con **peso totale massimo** (non solo quantità).
- **Esempio:** campagne pubblicitarie con ritorni economici, turni lavorativi remunerati.

**Perché greedy non funziona più?**

- Potresti dover scegliere meno intervalli, ma con somma di pesi più alta.

**Soluzione:**

- **Programmazione dinamica**:
    - Ordina i job per tempo di fine.
    - Per ciascun job j, calcola la soluzione ottima considerando:
        - Escludere j.
        - Includere j e aggiungere il peso + soluzione ottima dei job compatibili prima di lui.
- Complessità: O(nlogn).

---

### 🔹 Slide 29 – Bipartite Matching

- **Input:** grafo bipartito G=(U,V,E).
- **Goal:** trovare un matching massimo (numero massimo di coppie).
- **Applicazioni:**
    - Lavoratori ↔ Compiti.
    - Ospedali ↔ Residenti (estensione).
    - Assegnazione di studenti a progetti.

**Soluzione base:**

- Riduzione al **Max Flow Problem** → algoritmo di Edmonds-Karp / Dinic.
- Complessità efficiente: tipicamente O(n2m) o meglio.

---

### 🔹 Slide 30 – Maximum Independent Set

- **Definizione:** dato un grafo G, trovare il sottoinsieme più grande di nodi tale che nessuna coppia sia connessa da un arco.
- **Equivalente:** massimo numero di “persone” che non hanno conflitti diretti.

**Difficoltà:**

- È un problema **NP-completo**.
- Significa che non conosciamo algoritmi polinomiali e non ci aspettiamo di trovarne (a meno di P=NP).

**Applicazioni tipiche:**

- Pianificazione senza conflitti, codifica senza interferenze, problemi di rete.

---

### 🔹 Slide 31 – Competitive Facility Location

- **Scenario di gioco:** due giocatori scelgono nodi su un grafo, alternandosi.
- Regola: non puoi scegliere un nodo se un vicino è già stato preso.
- **Goal:** ogni giocatore vuole massimizzare il valore totale dei nodi presi.

**Osservazioni:**

- Modello competitivo di “facility placement” (es. aziende di fast food che vogliono aprire negozi evitando concorrenza troppo vicina).
- Dal punto di vista computazionale: **PSPACE-complete** → ancora più difficile di NP-complete, perché richiede ragionare su sequenze di mosse e strategie.

---

### 🔹 Slide 32–33 – Lezione generale

- Questi problemi mostrano la varietà delle difficoltà computazionali:

|Problema|Soluzione|Complessità|
|---|---|---|
|Interval Scheduling|Greedy|O(nlogn)|
|Weighted Interval Scheduling|DP|O(nlogn)|
|Bipartite Matching|Max Flow|Polinomiale|
|Independent Set|—|NP-completo|
|Competitive Facility Location|—|PSPACE-completo|

👉 Messaggio chiave: alcuni problemi hanno algoritmi eleganti ed efficienti, altri sono intrinsecamente difficili.

---

Vuoi che ti prepari una **mappa schematica comparativa** (tipo tabella visiva o diagramma) che riassuma differenze, algoritmi e difficoltà da slide 26 a 33?