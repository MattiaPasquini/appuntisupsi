# Grafica

**Data lezione**: ??

**Autori**: Pasqui

**Disclaimer**: non è ancora finito

[back](./../index.md)
[next](./03_math_computer_graphics_2.md)

# Matematica per computer grafica parte 1

Se non hai voglia di leggere le cose banali dove ti spiegano cos'è un punto, cos'è un vettore, ecc, puoi skippare fino [qui](#Libreria%20GLM%20(OpenGL%20Mathematics))

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


## 2. Unità di misura
Non c'ề un vero e proprio unità di misura fisico (es. $m$ (metri) ecc).
Le conversioni in pixel e la gestione di aspect ratio (4:3, 16:9 ecc.) avviene alla fine della pipeline di rendering.

## 3. Punti
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
## 4. Vettore
Un vettore rappresenta uno spostamento, infatti ha direzione e lunghezza (modulo) ma non la posizione.

Tipicamente in grafica si usa per:
- Direzione di un raggio
- Direzione e intensità di una luce
- Spostamento di un oggetto, velocità, accelerazione,...
- Normale di superficie (orientazione)
- Vento, forze, ecc.

### Convenzione
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

Oppure

$$
\vec{x} = \begin{bmatrix}
4 & 3 & 2
\end{bmatrix}
$$

$$
\textbf{x} = \begin{bmatrix}
4 & 3 & 2
\end{bmatrix}
$$

$$
\bar{x} = \begin{bmatrix}
4 & 3 & 2
\end{bmatrix}
$$

(Anche se mi sembra strano lol)
### Vettore nullo
Indicato ocn $\textbf{0}$ (Zero in grassetto) oppure $\vec{0}$:
$$
\vec{0} = \begin{bmatrix}
0 \\
0 \\
0
\end{bmatrix}
$$

Esempi:
$$
\vec{a} + \vec{0} = \vec{a}
$$

$$
\vec{b} - \vec{0} = \vec{b}
$$
### Operazioni tra vettori
#### Somma:
$$
\vec{a} + \vec{b} = \vec{b} + \vec{a} = \begin{bmatrix}
a_{1} + b_{1} \\
a_{2} + b_{2} \\
\dots \\
a_{n} + b_{n}
\end{bmatrix}
$$

#### Sottrazione
$$
\vec{a} - \vec{b} = \begin{bmatrix}
a_{1} - b_{1} \\
a_{2} - b_{2} \\
\dots \\
a_{n} - b_{n}
\end{bmatrix}
$$

$$
\vec{a} - \vec{b} \neq \vec{b} - \vec{a}
$$

$$
-\vec{a} = \begin{bmatrix}
-a_{1} \\
-a_{2} \\
\dots \\
-a_{n}
\end{bmatrix}
$$
#### Lunghezza del vettore
$$
\lvert a \rvert = \sqrt{ a_{1}^2 + a_{2}^2 + \dots + a_{n}^2}
$$

#### Vettore normalizzata
$$
\hat{a} = \begin{bmatrix}
\frac{a_{1}}{\lvert a \rvert } \\
\frac{a_{2}}{\lvert a \rvert } \\
\dots \\
\frac{a_{n}}{\lvert a \rvert }
\end{bmatrix}
$$

#### Prodotto scalare
$$
\vec{a} \cdot \vec{b} = \vec{b} \cdot \vec{a} = \sum_{i=1}^n a_{i}b_{i} = a_{1}b_{1} + a_{2}b_{2} + \dots + a_{n}b_{n}
$$

$$
\vec{a} \cdot \vec{0} = 0 = \vec{0} \cdot \vec{0}
$$

Se $\vec{a}$ e $\vec{b}$ fossero normalizzate, allora $\cos^{-1}(\vec{a} \cdot \vec{b})$ è l'angolo tra i due vettori.

#### Prodotto vettoriale (per vettori in $R^3$)
$$
a \times b = \begin{bmatrix}
a_{2}b_{3} - a_{3}b_{2} \\
a_{3}b_{1} - a_{1}b_{3} \\
a_{1}b_{2} - a_{2}b_{1}
\end{bmatrix}
$$

$$
\vec{a} \times \vec{b} \neq \vec{b} \times \vec{a}
$$

$$
\vec{a} \times \vec{a} = \vec{0}
$$


Sappiamo ovviamente che $\vec{a} \times \vec{b}$ è perpendicolare tra i due vettori.

## 5. Matrice
### Convenzione
$$
M = \begin{bmatrix}
m_{11} & m_{12} & m_{13} \\
m_{21} & m_{22} & m_{23} \\
m_{31} & m_{32} & m_{33}
\end{bmatrix}
$$

- **Matrice quadrata**: stesso numero di righe e colonne
- **Diagonale principale**: elementi con $i = j$ ($m_{11}, m_{22}, m_{33}$)

### Operazioni con matrici



## Libreria GLM (OpenGL Mathematics)
```cpp
#include <glm/glm.hpp>

int foo()
{
    glm::vec3 a(0.0f, 1.0f, -1.0f);
    glm::vec3 b = glm::vec3(1.0f);
    glm::vec3 c = a + b;
    glm::vec3 d = glm::cross(a, c);
    glm::vec4 r = glm::vec4(d, 1.0f);

    glm::mat3 M = glm::mat3(1.0f); // Matrice identità
    M[2] = c;                      // Terza colonna = c
    glm::mat3 Z(0);                // Matrice zero
    M = M * Z;                     // Risultato = matrice zero

    return 0;
}
```