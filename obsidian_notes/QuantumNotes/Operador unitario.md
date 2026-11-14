
# Operador unitario

> **En una frase:** un operador unitario es una matriz que **transforma estados sin romper nada**: conserva las normas y los productos internos, y siempre se puede deshacer. Es la forma en que **evoluciona** un sistema cuántico cerrado.

Relacionado: [[Operador hermítico]] - [[Producto externo]] - [[Base ortonormal]] - [[normalizado | Norma de un ket]]

---

## 1. Definición
Un operador $\hat U$ es unitario si cumple:

$$\hat U^\dagger \hat U = \hat U \hat U^\dagger = \hat I$$

- $\hat I$ es el **operador identidad** (unos en la diagonal, ceros en el resto). No es la $i$ imaginaria.
- $\hat U^\dagger$ es el **adjunto**: la **transpuesta conjugada** (transponés **y** conjugás cada número complejo). Con matrices reales coincide con la transpuesta; con complejas no.
- Consecuencia: $\hat U^{-1}=\hat U^\dagger$. El adjunto **deshace** lo que hizo $\hat U$.

## 2. Tres formas equivalentes de decir lo mismo

| # | propiedad | qué significa |
|---|---|---|
| 1 | $U^\dagger U = I$ | el adjunto es la inversa |
| 2 | $\langle U\varphi \vert U\psi\rangle = \langle \varphi\vert\psi\rangle$ | conserva productos internos (y por lo tanto normas y ortogonalidad) |
| 3 | las columnas de $U$ forman una [[Base ortonormal]] | cada columna mide 1 y son ortogonales entre sí (lo mismo vale para las filas) |

## 3. Qué hace (y qué NO hace) con un ket

- **Cambia** el ket: $U|\psi\rangle \neq |\psi\rangle$ en general.
- **Conserva su norma**:
$$\langle\psi|U^\dagger U|\psi\rangle=\langle\psi|\psi\rangle=1$$

Por eso un estado válido ($|\alpha|^2+|\beta|^2=1$) sigue siendo válido después de aplicarle $U$.

- **Se puede deshacer**: $U^\dagger\big(U|\psi\rangle\big)=|\psi\rangle$.

> [!warning] Error común
> "Unitario" **no** significa "devuelve el mismo ket". Cambia el ket pero conserva su longitud. El que devuelve el mismo ket es el **par** $U^\dagger U$.

## 4. Intuición

- Un unitario es una **rotación generalizada** en $\mathbb{C}^n$: gira los vectores sin estirarlos ni achicarlos.
- En un solo qubit, es una **rotación de la [[Esfera de Bloch]]** (salvo una [[Fase global y relativa|fase global]]).
- Es **reversible**: no pierde información. Por eso una **medición** (un proyector) **no** es unitaria.
- Conexión con Schrödinger: si $\hat H$ es [[Operador hermítico|hermítico]], entonces $U=e^{-i\hat H t/\hbar}$ es unitario. Los observables (hermíticos) generan la evolución (unitarios).

## 5. Ejemplos que SÍ son unitarios

**Compuertas básicas**

$$X=\begin{bmatrix}0&1\\1&0\end{bmatrix}\quad
Y=\begin{bmatrix}0&-i\\i&0\end{bmatrix}\quad
Z=\begin{bmatrix}1&0\\0&-1\end{bmatrix}\quad
H=\frac{1}{\sqrt2}\begin{bmatrix}1&1\\1&-1\end{bmatrix}$$

**Compuertas de fase**

$$S=\begin{bmatrix}1&0\\0&i\end{bmatrix}\qquad T=\begin{bmatrix}1&0\\0&e^{i\pi/4}\end{bmatrix}$$

Chequeo de $S$: $S^\dagger S=\begin{bmatrix}1&0\\0&(-i)(i)\end{bmatrix}=\begin{bmatrix}1&0\\0&1\end{bmatrix}=I$ ✓ (acá se ve por qué la conjugación es esencial: la transpuesta sola daría $i^2=-1$).

**El del ejercicio 16**

$$U_1=\frac{1}{\sqrt2}\begin{bmatrix}1&i\\i&1\end{bmatrix}$$

Aplicado a $|+\rangle$ da $e^{i\pi/4}|+\rangle$: el ket cambia numéricamente, pero es solo una fase global.

## 6. Ejemplos que NO son unitarios

| matriz | por qué falla |
|---|---|
| $\begin{bmatrix}1&0\\0&2\end{bmatrix}$ | $A\vert1\rangle=2\vert1\rangle$: la norma pasa de 1 a 2 |
| $\begin{bmatrix}1&i\\i&1\end{bmatrix}$ (sin el $\frac{1}{\sqrt2}$) | cada columna mide $\sqrt2$, no 1. El factor $\frac{1}{\sqrt2}$ es un **factor de normalización** |
| $\vert0\rangle\langle0\vert=\begin{bmatrix}1&0\\0&0\end{bmatrix}$ | proyector: $P^\dagger P=P\neq I$, y pierde información |

## 7. Propiedades útiles

- **El producto de unitarios es unitario**:

$$(U_1U_2)^\dagger(U_1U_2)=U_2^\dagger\underbrace{U_1^\dagger U_1}_{I}U_2=U_2^\dagger U_2=I$$

  (el adjunto de un producto invierte el orden).
- **El inverso de un unitario es unitario**: $U^\dagger$ también lo es.
- **Multiplicar por una fase global** no rompe nada: $e^{i\varphi}U$ sigue siendo unitario.
- **Aplicado a una base ortonormal da otra base ortonormal**: si $\{|u_i\rangle\}$ lo es, $|w_i\rangle=U|u_i\rangle$ también (ejercicio 18). Además, $U|0\rangle$ y $U|1\rangle$ son justamente la primera y la segunda columna de $U$.
- **Los autovalores tienen módulo 1**: si $Uv=\lambda v$, entonces $\|v\|=\|Uv\|=|\lambda|\,\|v\|$, así que $|\lambda|=1$.

## 8. Unitario vs. hermítico

| | **Hermítico** | **Unitario** |
|---|---|---|
| condición | $A^\dagger=A$ | $U^\dagger U=I$ |
| rol | observable (lo que se mide) | evolución (lo que transforma el estado) |
| autovalores | reales | módulo 1 |

$X,Y,Z,H$ son **las dos cosas a la vez**. $S$ y $T$ son unitarios pero **no** hermíticos.

## 9. En Qiskit / NumPy

```python
from qiskit.quantum_info import Operator, Statevector
import numpy as np

U = Operator(matriz)          # matriz: np.array cuadrado
U.is_unitary()                # verifica U†U = I
U.adjoint()                   # el adjunto (devuelve un operador nuevo)
U.dot(V)                      # producto U·V   (compose daría V·U)
estado.evolve(U)              # NO modifica 'estado': devuelve uno nuevo
np.allclose(U.data.conj().T @ U.data, np.eye(2))   # chequeo a mano
```

- Las operaciones de Qiskit **devuelven objetos nuevos**; hay que guardar el resultado.
- Los errores de redondeo (`1e-17`) son normales: comparar con `np.isclose` / `np.allclose`.

## 10. Dónde lo usé
- Ejercicio 16: verificar $U_1$ unitario y que conserva la norma.
- Ejercicio 17: el producto $U_3=U_1U_2$ es unitario.
- Ejercicio 18: $U_2$ transforma la base computacional en otra base ortonormal.
- Ejercicio 19: verificar $X,Y,Z,H,S,T$.
