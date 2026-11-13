### La idea
Una suma de productos externos es una matriz escrita en otro formato. Cada término $c\,|i\rangle\langle j|$ pone su coeficiente $c$ en la **fila $i$, columna $j$**. Todo lo que no aparece en la suma queda en 0.

### Pasos
1. Arma una grilla $2 \times 2$ (o $2^n \times 2^n$ para $n$ qubits) con ceros
2. Después para cada término, mira su ket y su bra:
	1. El [[ket]] dice la fila,
	2. El [[bra]] dice la columna
3. Después hay que **escribir el coeficiente en esa casilla**. Si el término no tiene número adelante, el coeficiente es 1. pero si tiene un signo, el signo es parte del coeficiente.
4. Se pasa a código con `np.array`, una lista por fila
5. Convertirlo a operador con `Operator(matriz)`
6. Después verificas con `.is_unitary()` si el enunciado habla de un unitario

### Ejemplo completo
$$\large{V=\tfrac{1}{2}|0\rangle\langle0|-i\,|0\rangle\langle1|+e^{i\pi/3}|1\rangle\langle0|+|1\rangle\langle1|}$$

| término                                     | ket $\rightarrow$ fila | bra $$\rightarrow columna | coeficiente          |
| ------------------------------------------- | ---------------------- | ------------------------- | -------------------- |
| $\frac12 \vert 0\rangle \langle 0\vert$     | 0                      | 0                         | `1/2`                |
| $-i \vert 0 \rangle \langle 1 \vert$        | 0                      | 1                         | `-1j`                |
| $e^{i\pi/3}\vert 1 \rangle \langle 0 \vert$ | 1                      | 0                         | `np.exp(1j*np.pi/3)` |
| $\vert 1 \rangle \langle 1 \vert$           | 1                      | 1                         | `1`                  |

```python
import numpy as np
from qiskit.quantum_info import Operator

V = np.array([[1/2,                    -1j],
              [np.exp(1j*np.pi/3),      1 ]])

V = Operator(V)
print(V.is_unitary())   # este V no es unitario: sirve solo para mostrar el formato
```

### Atajos y trampas

- **Fases:** $e^{i\theta}$ se escribe `np.exp(1j*theta)`, copiando el ángulo con su signo. **No** es `1j`, salvo que $\theta=\frac{\pi}{2}$.
- **Paréntesis:** si el ángulo tiene una resta o suma, va entre paréntesis: `np.exp(1j*(np.pi/5 - np.pi/7))`.
- **Matriz vs. `Operator`:** `Operator(...)` recibe **un solo argumento**, la matriz ya armada. No recibe ket ni bra por separado.
- **Orden de producto:** para $A\cdot B$ usa `A.dot(B)`. Con `A.compose(B)` obtenés $B\cdot A$.
- **Más de un qubit:** para $|ab\rangle\langle cd|$, la fila es el número binario $ab$ en decimal y la columna es $cd$ en decimal. Por ejemplo, $|01\rangle\langle10|$ va en fila 1, columna 2, en una matriz 4×4.

### Variante: si te dan los kets con amplitudes
Cuando el operador viene como producto externo de kets “armados” (como en el ejercicio 8), podés usar `np.outer` en vez de armar la matriz a mano:

```python
A = np.outer(ket, bra_data.conjugate())   # el segundo vector se conjuga a mano
op = Operator(A)
```

Si hay varios términos, pues sumas los `np.outer` entre sí.

### Al revés: de matriz a ket-bra
Cada elemento no nulo $a_{ij}$ de la matriz es el coeficiente de $|i\rangle\langle j|$. Eso también te sirve para chequear tu trabajo.