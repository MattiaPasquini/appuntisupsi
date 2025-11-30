# Grafica

**Data lezione**: ??

**Autori**: Pasqui

**Disclaimer**: non è ancora finito

[back](./../index.md)
[next](./03_math_computer_graphics_2.md)

# Matematica per computer grafica
## 1. Sistemi di coordinate

### Coordinate cartesiane
Ovviamente viene introdotto il sistema di coordinate $(x, y)$ per **2D** e $(x,y,z)$ e **3D**.

### Mano destra e mano sinistra
I sani di mente utilizzano la regola della mano destra (Right-hand rule)

![](./attachments/Pasted%20image%2020251130000838.png)

Alcune librerie usano convenzioni diverse (tipo DirectX usa Left-Hand rule) ma [OpenGL](https://www.opengl.org/) usa la regola Right-Hand rule (per fortuna lmao).
È importante perché le trasformazioni, illuminazione, ecc dipendono da questa convenzione

### Coordinate sullo schermo in 2D

Sistemi usati dalle GUI:
- Origine in alto a sinistra: Windows, X11 → x verso destra, y verso il basso.
- Origine in basso a sinistra: MacOS, OpenGL → x verso destra, y verso l’alto.
Serve per capire come mappare coordinate logiche in pixel sullo schermo.
![](./attachments/Pasted%20image%2020251130001728.png)


## Unità di misura
Non c'ề un vero e proprio unità di misura fisico (es. $m$ (metri) ecc).
Le conversioni in pixel e la gestione di aspect ratio (4:3, 16:9 ecc.) avviene alla fine della pipeline di rendering.

## Punti
Un punto rappresenta nella posizione nello spazio e non ha la lunghezza, spessore e direzione.

In grafica un punto è tipicamente un vertice di un oggetto in 3D ma può essere anche la posizione di una luce, di una camera, ecc.

Può essere scritto in questi modi

$$
(4,3,2)
$$
$$
\begin{bmatrix}
4 \\
3 \\
2
\end{bmatrix} = \begin{pmatrix}
4 \\
3 \\
2
\end{pmatrix}
$$
$$
\begin{bmatrix}
4 & 3 & 2
\end{bmatrix} ^ T
$$
## Vettore
Un vettore rappresenta uno spostamento, infatti ha direzione e lunghezza (modulo) ma non la posizione.

Tipicamente in grafica si usa per:
- Direzione di un raggio
- Direzione e intensità di una luce
- Spostamento di un oggetto, velocità, accelerazione,...
- Normale di superficie (orientazione)
- Vento, forze, ecc.

Un vettore può essere scritto in questi modi:
$$
\vec{x} = \begin{bmatrix}
4 \\
3 \\
2
\end{bmatrix}
$$
$$
\textbf{x} = \begin{bmatrix}
4 \\
3 \\
2
\end{bmatrix}
$$
$$
\bar{x} = \begin{bmatrix}
4 \\
3 \\
2
\end{bmatrix}
$$
