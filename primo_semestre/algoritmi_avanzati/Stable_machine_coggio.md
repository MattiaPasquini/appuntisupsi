## Esempio: Residenti ed Ospedali
- L'obiettivo è progettare un processo di ammissione autosostenibile, dato un'insieme di preferenze tra studenti di medicina ed ospedali.
- Una coppia (x=candidato, y=ospedale) è instabile se:
	- x preferisce y rispetto all'ospedale al quale è assegnato
	- y preferisce x rispetto ad uno dei candidati già ammessi
- Un'assegnazione stabile è una in cui non esistono coppie instabili
	- **condizione naturale e desiderabile**
## Stable Machine Classico
- Dato un numero uguale di uomini e donne,  bisogna trovare un'abbinamento "adeguato"
- Ogni partecipante ordina in una lista di preferenze tutti i membri dell'altro sesso
- Definizione: 
	- **Perfect matching**: ognuno ha un **parter** **unico**
	- **Stable machine**: **non** **esiste** una **coppia** **m**-**w** **che** **preferisce** **scappare** **insieme** **rispetto** **ai partner attuali**
## Stable Roommate problem
- Domanda: Esistono sempre matching stabili?
- Con 2n persone che si devono dividere in coppie di coinquilini, **non sempre esiste una soluzione stabile.**
- Esempio con A, B, C, D --> qualunque combinazione produce instabilità, perché?
- Preferenze:
	- **A**: B > C > D
	- **B**: C > A > D
	- **C**: A > B > D
	- **D**: A > B > C
- Possibili accoppiamenti:
	- **A-B, C-D**
	    - Ma **B** preferisce **C** a **A**, e **C** preferisce **B** a **D** → **coppia instabile (B,C)**.
	- **A-C, B-D**
	    - Ma **A** preferisce **B** a **C**, e **B** preferisce **A** a **D** → **coppia instabile (A,B)**.
	- **A-D, B-C**
	    - Ma **A** preferisce **C** a **D**, e **C** preferisce **A** a **B** → **coppia instabile (A,C)**.

## Algoritmo di Gale-Shapley (1962) -Propose-and-Reject
- Procedura intuitiva che garantisce di trovare un matching stabile:
	- Tutti iniziano liberi
	- Finchè esiste un'uomo libero che non ha proposto a tutte:
		- Propone alla prima in lista alla quale non ha ancora proposto
		- Se la donna è libera --> si fidanza
		- Se la donna è occupata ma preferisce lui --> cambia fidanzato e si mette con lui
		- Altrimenti lo rifiuta
```c
// gale_shapley.c
// Implementazione dell'algoritmo di Gale–Shapley (men-proposing)
// Compila:   gcc -O2 -Wall -Wextra -o gale_shapley gale_shapley.c
// Esegui:    ./gale_shapley < input.txt

#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

static void *xmalloc(size_t n) {
    void *p = malloc(n);
    if (!p) { perror("malloc"); exit(1); }
    return p;
}

int main(void) {
    int n;
    if (scanf("%d", &n) != 1 || n <= 0) {
        fprintf(stderr, "Errore: inserire n > 0\n");
        return 1;
    }

    // preferenze uomini: men_pref[m][k] = donna al k-esimo posto per l'uomo m
    int **men_pref = xmalloc(n * sizeof(*men_pref));
    for (int m = 0; m < n; ++m) {
        men_pref[m] = xmalloc(n * sizeof(**men_pref));
        for (int k = 0; k < n; ++k) {
            if (scanf("%d", &men_pref[m][k]) != 1) {
                fprintf(stderr, "Errore: leggi preferenze uomini\n");
                return 1;
            }
            if (men_pref[m][k] < 0 || men_pref[m][k] >= n) {
                fprintf(stderr, "Errore: indice donna fuori range\n");
                return 1;
            }
        }
    }

    // preferenze donne: women_pref[w][k] = uomo al k-esimo posto per la donna w
    int **women_pref = xmalloc(n * sizeof(*women_pref));
    for (int w = 0; w < n; ++w) {
        women_pref[w] = xmalloc(n * sizeof(**women_pref));
        for (int k = 0; k < n; ++k) {
            if (scanf("%d", &women_pref[w][k]) != 1) {
                fprintf(stderr, "Errore: leggi preferenze donne\n");
                return 1;
            }
            if (women_pref[w][k] < 0 || women_pref[w][k] >= n) {
                fprintf(stderr, "Errore: indice uomo fuori range\n");
                return 1;
            }
        }
    }

    // rank[w][m] = posizione (più piccolo = preferito) dell'uomo m nella lista della donna w
    int **rank = xmalloc(n * sizeof(*rank));
    for (int w = 0; w < n; ++w) {
        rank[w] = xmalloc(n * sizeof(**rank));
        for (int pos = 0; pos < n; ++pos) {
            int m = women_pref[w][pos];
            rank[w][m] = pos;
        }
    }

    // partner attuali (-1 se libero)
    int *woman_partner = xmalloc(n * sizeof(*woman_partner));  // donna -> uomo
    int *man_partner   = xmalloc(n * sizeof(*man_partner));    // uomo  -> donna
    for (int i = 0; i < n; ++i) { woman_partner[i] = -1; man_partner[i] = -1; }

    // prossimo indice di proposta per ciascun uomo (scorre la sua lista)
    int *next_idx = xmalloc(n * sizeof(*next_idx));
    for (int m = 0; m < n; ++m) next_idx[m] = 0;

    // coda semplice di uomini liberi
    int *queue = xmalloc(n * sizeof(*queue));
    int qh = 0, qt = 0;
    for (int m = 0; m < n; ++m) queue[qt++] = m;

    // Gale–Shapley O(n^2)
    while (qh < qt) {
        int m = queue[qh++];                // uomo libero
        if (man_partner[m] != -1) continue; // già accoppiato (difensivo)

        if (next_idx[m] >= n) {
            fprintf(stderr, "Attenzione: l'uomo %d ha esaurito le proposte.\n", m);
            continue; // in teoria non accade in matching bipartito completo
        }

        int w = men_pref[m][next_idx[m]++]; // propone alla prossima donna in lista

        if (woman_partner[w] == -1) {
            // donna libera: si fidanzano
            woman_partner[w] = m;
            man_partner[m]   = w;
        } else {
            int m2 = woman_partner[w]; // attuale fidanzato di w
            if (rank[w][m] < rank[w][m2]) {
                // w preferisce m a m2: scambio partner
                woman_partner[w] = m;
                man_partner[m]   = w;
                man_partner[m2]  = -1;
                // m2 torna libero: rimetti in coda
                queue[--qh] = m2; // push-front per accelerare (va bene anche push-back)
            } else {
                // w rifiuta m: m resta libero -> rientra in coda se ha ancora opzioni
                if (next_idx[m] < n) {
                    queue[qt++] = m;
                }
            }
        }
    }

    // Stampa matching uomo->donna
    // Ogni riga: "m w" indica che l'uomo m è abbinato alla donna w.
    for (int m = 0; m < n; ++m) {
        if (man_partner[m] == -1) {
            fprintf(stderr, "Uomo %d non abbinato (input non corretto?).\n", m);
        } else {
            printf("%d %d\n", m, man_partner[m]);
        }
    }

    // cleanup (facoltativo per programma breve)
    for (int m = 0; m < n; ++m) free(men_pref[m]);
    for (int w = 0; w < n; ++w) { free(women_pref[w]); free(rank[w]); }
    free(men_pref); free(women_pref); free(rank);
    free(woman_partner); free(man_partner); free(next_idx);
    free(queue);
    return 0;
}

```


## Domande Wooclap
	1)
	2)
	3) # Come funziona l'algoritmo di Gale-Shapley? --> Gli uomini propongono alle donne, e le donne accettano o rifiutano in base alle loro preferenze.
	4) # Qual è una delle proprietà fondamentali della soluzione stabile nell'algoritmo di Gale-Shapley? --> L'algoritmo termina sempre con il matching perfetto/stabile. / Il matching massimizza il numero di coppie.
	5) # Cosa succede se l'algoritmo di Gale-Shapley termina? --> Il matching trovato è sempre stabile.
	6) # Quale delle seguenti affermazioni è falsa riguardo allo stable matching? --> Il matching stabile è unico.
