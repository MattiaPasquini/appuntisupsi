## Valore temporale
C'è un concetto importante da tenere in mente: **Un dollaro di oggi non equivale a un dollaro di domani**. Perché nel tempo il denaro ha un costo opportunità: le risorse investite oggi potrebbe essere impiegate in alternative che generano un rendimento.

Ci sono vari motivi del **valore temporale**:
- Opportunità di investimento: un capitale può essere reinvestito e produrre interessi.
	- 
- Inflazione: Il potere d'acquisto della moneta tende a ridursi nel tempo.
- Rischio e incertezza: flussi futuri non sono mai sicuri al 100%, quindi vanno “scontati” per
riflettere il rischio.

Valore attuale di una serie di flussi (_Present Value of a Stream_) utile quando i flussi futuri sono ripetuti per più anni.
$$
PV = \sum_{t=1}^n \frac{FV_{t}}{(1+i)^t}
$$
Dove $FV$ è valore futuro, $i$ è il tasso di interesse/attualizzazione e $n$ sono i numeri di periodi.
Il valore attuale netto (_Net Present Value, $NPV$_) è la differenza tra PV dei benifici futuri e costo iniziale dell'investimento ($C_{0}$)
$$
NPV = PV - C_{0}
$$
Con $NPV$ puoi determinare la decisione (Regola decisionale, Decision Rule):
- Se $NPV > 0$  --> il progetto crea valore per l'impresa --> accettare 
- Se $NPV < 0$ --> il progetto distrugge valore --> rifiutare

NPV è uno strumento importante perché tiene conto sia dell’entità dei flussi, sia della loro
tempistica e del costo opportunità del capitale.

### Esempio
- Investimento iniziale: $C_{0} = 20$ milioni di dollari
- Flussi di cassa previsti: $FV_{t} = 7$ milioni di dollari
- Tasso di attualizzazione: $i = 7\%$ (costo di opportunità del capitale)

Calcolo $PV$:
$$
PV = \frac{7}{(1 + 0,07)^1} + \frac{7}{(1 + 0,07)^2} + \frac{7}{(1 + 0,07)^3} \approx 18,37 \text{ milioni di dollari}
$$
Valore attuale netto $NPV$:
$$
NPV = PV - C_{0} = 18,37 - 20 = -1,63 \text{ milioni di dollari}
$$

### Attenzione!
Ovviamente non conta solo i numeri, bisogna stare comunque attenti ai rischi e qualità dei dati, bisogna saper stimare bene i flussi di cassa futuri e gestire bene la contabilità.

## Tre approcci per capire le scelte aziendali
Per capire le logiche delle decisioni aziendali è necessario distinguere tre approcci:
- La Microeconomia, che offre i modelli teorici di base
	- Studia il comportamento di consumatori e imprese in mercati concorrenziali o meno.
	- Analizza meccanismi di domanda e offerta, formazione dei prezzi, equilibrio di mercato.
	- È una disciplina teorica e astratta, con l’obiettivo di spiegare regolarità economiche generali.
- L'Econonomia aziendale, che descrive e interpreta come sistema
	- Studia l’azienda come sistema economico, in tutte le sue dimensioni (produzione, finanza, organizzazione, strategia).
	- Si concentra sul funzionamento concreto dell’impresa e delle altre istituzioni economiche (banche, pubblica amministrazione, non-profit).
	- Approccio olistico e descrittivo, utile a comprendere come le imprese operano e competono.
- L'Economia Manageriale, che utilizza teoria e strumenti per guidare le scelte concrete dei manager sotto vincoli.
	- Disciplina di confine: applica strumenti e modelli della microeconomia per risolvere problemi di gestione aziendale.
	- Focalizzata sul processo decisionale del manager: come scegliere la strategia ottimale in presenza di vincoli e incertezza.
	- Ha un taglio più analitico e quantitativo rispetto all'economia aziendale, ma è più applicata e concreta rispetto alla microeconomia pura.

## Sette idee da ricordare
1. **Identificare obiettivi e vincoli**  
   • Ogni decisione nasce dall’individuare ciò che si vuole ottenere e i limiti entro cui ci si deve muovere.

2. **Riconoscere la natura e l’importanza dei profitti**  
   • I profitti sono la misura chiave della creazione di valore e il segnale che orienta risorse e investimenti.

3. **Comprendere gli incentivi**  
   • Le persone e le aziende rispondono agli incentivi: per ottenere i risultati desiderati, occorre disegnarli con attenzione.

4. **Comprendere i mercati**  
   • Le aziende operano in mercati caratterizzati da concorrenza, clienti, fornitori e intervento pubblico.

5. **Riconoscere il valore temporale del denaro**  
   • Un CHF oggi non equivale a un CHF domani: i flussi futuri vanno sempre attualizzati.

6. **Usare l’analisi marginale**  
   • Per prendere decisioni ottimali è utile ragionare «al margine», confrontando benefici e costi incrementali.

7. **Prendere decisioni con l’aiuto dei dati**  
   • L’uso consapevole delle informazioni e dei metodi quantitativi migliora la qualità delle scelte manageriali.
