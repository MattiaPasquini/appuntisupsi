# Interval Scheduling

## Brute Force
Se usassimo la brute force, la complessità sarebbe $O(2^n)$. Perché ogni caso ho due scelte che sono "scelgo" e "non scelgo". La complessità è troppo brutta quindi sarebbe meglio evitare

## Greedy
Ci sono tante strategie:
- Prendere un j\*b  che inizia per prima
- Prendere un j\*b che finisce più tardi
- L'intervallo di j\*b più corto
- Iniziare da quello che ha meno conflitti

Ma nessuno di questi sono ottimali