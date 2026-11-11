En álgebra lineal y mecánica cuántica, decimos que un vector $\vert{}\psi\rangle$ es un **autovector** (o _eigenstate_) de un operador $\hat{A}$ si, al aplicarle el operador, **el estado no cambia de dirección u orientación**, sino que como máximo solo se multiplica por un número (un escalar $\lambda$):

$$\hat{A}\vert{}\psi\rangle = \lambda \vert{}\psi\rangle$$

Ese número $\lambda$ se conoce como **autovalor** (_eigenvalue_).

### Viéndolo con los datos de código 
Fíjate en lo que ocurrió cuando le aplicamos el operador $X$ a la base de Hadamard:
1. **Para $\vert{}+\rangle$:**
$$
X \vert{}+\rangle = [0.7071, 0.7071] = 1 \cdot \vert{}+\rangle
$$
    - El estado quedó **exactamente igual**. Su autovalor es $+1$.
2. **Para $\vert{}-\rangle$:**
$$
X \vert{}-\rangle = [-0.7071, 0.7071] = -1 \cdot [0.7071, -0.7071] = -1 \cdot \vert{}-\rangle
$$
    - El estado sigue siendo $\vert{}-\rangle$, solo que multiplicado por $-1$ (una fase). Su autovalor es $-1$.

En cambio, cuando aplicamos $X$ a $\vert{}0\rangle$, cambió totalmente a $\vert{}1\rangle$. ¡Por eso $\vert{}0\rangle$ **no** es autovector de $X$