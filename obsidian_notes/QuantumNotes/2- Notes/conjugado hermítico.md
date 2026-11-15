**En una frase:** es **transponer y conjugar** (cambiar cada $i$ por $-i$). El orden de las dos operaciones no importa.
$$A^\dagger=\overline{A}^{,T}$$
## Sobre qué se aplica

| objeto | resultado    | ejemplo                                                                                                 |
| ------ | ------------ | ------------------------------------------------------------------------------------------------------- |
| ket    | **bra**      | $\begin{bmatrix}\alpha\\beta\end{bmatrix}^\dagger=\begin{bmatrix}\bar\alpha&\bar\beta\end{bmatrix}$     |
| bra    | ket          | $\langle\psi\vert^\dagger=\vert\psi\rangle$                                                             |
| matriz | otra matriz  | $\begin{bmatrix}a&b\\c&d\end{bmatrix}^\dagger=\begin{bmatrix}\bar a&\bar c\\\bar b&\bar d\end{bmatrix}$ |
| número | su conjugado | $(2+3i)^\dagger=2-3i$                                                                                   |

Con **matrices reales**, el adjunto es solo la transpuesta.

**Un ejemplo hecho paso a paso:**
$$\begin{bmatrix}1&i\\2&3\end{bmatrix}\xrightarrow{\text{transponer}}\begin{bmatrix}1&2\\i&3\end{bmatrix}\xrightarrow{\text{conjugar}}\begin{bmatrix}1&2\\-i&3\end{bmatrix}$$
## Propiedades que más se usan
- $(A^\dagger)^\dagger=A$
- $(AB)^\dagger=B^\dagger A^\dagger$: **se invierte el orden**.
- $(cA)^\dagger=\bar c,A^\dagger$: el escalar se conjuga.
- $(U|\psi\rangle)^\dagger=\langle\psi|U^\dagger$: es la regla con la que se demuestra que un unitario conserva la norma.
- $(A\otimes B)^\dagger=A^\dagger\otimes B^\dagger$: con el producto tensorial **no** se invierte el orden. Está en el notebook de la Clase 2.
- $(|v\rangle\otimes|w\rangle)^\dagger=\langle v|\otimes\langle w|$.

Un ejemplo con la fórmula del notebook, $Y=e^{i\pi/2}XZ$: $Y^\dagger=e^{-i\pi/2}Z^\dagger X^\dagger=-i,ZX$. Fíjate que el orden se invirtió y la fase se conjugó. Y como $-iZX=Y$, resulta $Y^\dagger=Y$.

## Dónde aparece

|concepto|cómo usa el adjunto|
|---|---|
|bra|$\langle\psi|
|producto interno y norma|$\langle\psi|
|producto externo|$|
|operador **hermítico**|$A^\dagger=A$ (observables: $X,Y,Z,H$)|
|operador **unitario**|$U^\dagger U=I$ (compuertas, evolución)|
|reversibilidad|$U^\dagger$ **deshace** lo que hizo $U$|

## Errores comunes

- **Olvidar conjugar:** transponer solo da el resultado correcto únicamente con matrices reales. Es el error que aparece con $S=\mathrm{diag}(1,i)$.
- **No invertir el orden** en $(AB)^\dagger$.
- **Confundir hermítico con adjunto:** el adjunto es la operación, y hermítico es una propiedad ($A^\dagger=A$).

## En código
- **NumPy:** `M.conj().T`
- **Qiskit:** `Operator.adjoint()` devuelve el adjunto de un operador; `.conjugate()` y `.transpose()` hacen cada paso por separado, y el vector de amplitudes de un bra sale de `sv.conjugate()` (lo usaste en el ejercicio 3).
