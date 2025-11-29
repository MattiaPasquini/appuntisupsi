Excellent — let’s take a deep dive into **DSSR (Decremental State Space Relaxation)**, keeping the **formal structure** and terminology used in the paper, but explaining clearly what each part *means* and *why it matters*.

We’ll go step by step, mirroring the original section from the paper but adding explanations in plain English.

---

## 🧠 What DSSR Is

**Decremental State Space Relaxation (DSSR)** is a **new algorithm** introduced in the paper to solve the **Resource-Constrained Elementary Shortest Path Problem (RCESPP)** efficiently.

It’s called *decremental* because it **gradually reduces (or “shrinks”) the relaxed search space**, until the algorithm finds an *elementary* (cycle-free) path.

It sits **between** two extremes:

* At one end, **exact dynamic programming (DP)** enforces that no vertex is ever visited twice (completely elementary paths).
* At the other end, **state space relaxation (SSR)** allows nodes to be revisited (non-elementary paths).

DSSR **starts like SSR**, allowing some node repetitions, and then **step by step**, it prevents more and more of them, until all paths are elementary.

---

## 🧩 The Core Idea

Let’s define:

* ( Θ ): the set of **critical vertices** — vertices that are *not allowed* to be visited more than once.
* ( Ψ ): the set of **vertices that were visited more than once** in the *current* best solution.

The algorithm works iteratively:

1. Start with ( Θ = ∅ ) (no vertex is critical — all can be repeated).
2. Run a dynamic programming search with this relaxed rule.
3. Look at the solution.

   * If some vertices appear more than once (so ( Ψ \neq ∅ )), mark those vertices as **critical**.
4. Update the critical set:
   ( Θ ← Θ ∪ Ψ )
5. Run the algorithm again, this time forbidding multiple visits to the critical vertices.
6. Repeat until no vertex is visited more than once (i.e. ( Ψ = ∅ )).

At that point, you have an **elementary path** that is **optimal**.

---

## ⚙️ What Happens Inside Each Iteration

Each DSSR iteration runs a **bounded bi-directional dynamic programming algorithm**, similar to what was described earlier in the paper.
Let’s break down the main components of that algorithm.

---

### 1. **State Definition**

Each *state* (or “label”) represents a partial path from the start vertex ( s ) to some vertex ( i ).

A state in DSSR is written as:
[
(S_Θ, R, C, i)
]

Where:

* ( S_Θ ): vector tracking visits **only to the critical vertices** in ( Θ ).

  * For a vertex ( k \in Θ ), ( S_Θ(k) = 1 ) means it has already been visited.
  * Non-critical vertices are *not tracked* (so they can still be visited multiple times).
* ( R ): vector of **resources consumed** (e.g., capacity, time, etc.).
* ( C ): **total cost** accumulated so far.
* ( i ): **current vertex**.

This is like a “snapshot” of one possible route in progress.

💡 *Note:*
In standard DP, ( S ) tracks all vertices (so it’s exponentially large).
In DSSR, ( S_Θ ) only tracks *some* of them — this drastically reduces the number of possible states.

---

### 2. **State Extension**

From a given state ( (S_Θ, R, C, i) ), you can **extend** it along an outgoing edge ((i, j)) to create a new state ( (S'_Θ, R', C', j) ), as long as:

* The move obeys resource constraints (e.g., capacity ( ≤ Q ), time ( ≤ b_i )).
* If ( j \in Θ ), then ( S_Θ(j) = 0 ) (not yet visited).
  If ( j \notin Θ ), you don’t check this.

Mathematically:
[
S'_Θ(k) =
\begin{cases}
S_Θ(k) + 1 & \text{if } k = j \text{ and } j \in Θ\
S_Θ(k) & \text{otherwise}
\end{cases}
]

The cost and resources are updated according to the same rules used in earlier algorithms:
[
C' = C - \frac{λ_i}{2} + c_{ij} - \frac{λ_j}{2}
]
and
[
R' = f(R, i, j)
]
where ( f ) represents the specific update for resources (like capacity or time).

---

### 3. **Dominance Test**

To avoid keeping redundant states, DSSR applies a **dominance rule**:
If you have two states ending at the same vertex ( i ):
[
(S_Θ', R', C') \text{ and } (S_Θ'', R'', C'')
]
then the first **dominates** the second if:

* ( S_Θ' ≤ S_Θ'' )
* ( R' ≤ R'' )
* ( C' ≤ C'' )
  and at least one inequality is strict.

Dominated states can be discarded because they cannot lead to a better final solution.

---

### 4. **Joining Forward and Backward Paths**

DSSR uses **bi-directional search**, meaning it explores:

* Forward paths (from ( s ) to intermediate vertices),
* Backward paths (from ( t ) backward to intermediate vertices).

Whenever a forward and backward state can be joined **feasibly** (resources and visits match up), the algorithm forms a complete ( s-t ) path.

To avoid duplicate solutions, it uses a **“halfway” rule**:
Only accept paths where the forward and backward resource usage are as close as possible to half of the total.

---

### 5. **Checking for Multiple Visits**

After each full iteration, the algorithm looks at the best path found and records which vertices were visited more than once:
[
Ψ ← \text{MultipleVisits()}.
]

Those vertices become *critical* in the next iteration:
[
Θ ← Θ ∪ Ψ.
]

If ( Ψ = ∅ ), the path is elementary and optimal — the algorithm stops.

---

## 🔁 The Algorithm in Pseudo-Code (with explanations)

Here’s the formal pseudocode from the paper, rewritten with commentary:

```text
Algorithm DSSR (RCESPP)

Ψ ← ∅              // Vertices visited more than once (initially empty)
Θ ← ∅              // Set of critical vertices (initially empty)

repeat
    Θ ← Θ ∪ Ψ      // Add new critical vertices
    Initialize forward and backward label sets Γ_fw and Γ_bw
    E ← {s, t}     // Vertices to explore (start and end)

    repeat
        Select vertex i ∈ E
        // Forward expansion
        For each label l_i in Γ_fw[i]:
            For each successor j of i:
                if (j ∉ Θ) or (SΘ[j] = 0):   // can visit j if it's not critical, or not yet visited
                    Create label l_j by extending l_i
                    Add l_j to Γ_fw[j] if non-dominated
                    If Γ_fw[j] changed, add j to E

        // Backward expansion (similar but in reverse)
        For each label l_i in Γ_bw[i]:
            For each predecessor k of i:
                if (k ∉ Θ) or (SΘ[k] = 0):
                    Create label l_k by extending l_i
                    Add l_k to Γ_bw[k] if non-dominated
                    If Γ_bw[k] changed, add k to E

        Remove i from E
    until E = ∅

    // Combine forward and backward paths into complete solutions
    For each feasible pair (l_fw, l_bw):
        If Feasible and Halfway(l_fw, l_bw):
            Save solution

    // Find vertices visited more than once
    Ψ ← MultipleVisits()

until Ψ = ∅
```

---

## 🧩 What Makes DSSR Efficient

* **Reduced state space:**
  It tracks visits only for “problematic” vertices — not all vertices.
  This makes it exponentially smaller than exact DP.

* **Iterative tightening:**
  Each run starts easy and becomes stricter only where needed.

* **Convergence guarantee:**
  Since ( Θ ) grows monotonically (more and more vertices become critical), the algorithm must end — in the worst case, it becomes the full exact DP.

---

## 📊 Experimental Behavior

* Usually converges in **fewer than 15 iterations**.
* The number of critical vertices ( |Θ| ) stays small.
* Often **two orders of magnitude faster** than pure DP.
* Very simple to implement compared to branch-and-bound.

---

## 🧭 Summary of DSSR in One Sentence

> **DSSR** solves the RCESPP by starting with a relaxed problem that allows node repetitions and gradually restricting only those nodes that cause cycles, until it reaches an exact, elementary optimal path — balancing accuracy, simplicity, and computational speed.

---

Would you like me to illustrate DSSR with a **concrete example**, like a tiny network (say, 4–5 nodes) showing how Θ and Ψ evolve across iterations? That’s often the best way to see how the algorithm “tightens” the solution.