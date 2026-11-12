# Por si queres releer la teoría
[[Clase 1 Parte 2]]

# Ejercicios

## Ejercicio 11: Proyectando qubits
a. Crea el proyector $\hat{P}_0$ asociado al estado $|\psi_0\rangle=\frac{i}{\sqrt{3}}|0\rangle+\sqrt{\frac{2}{3}}|1\rangle$ y verifica que cumple con las ecuaciones $\hat{P}^2=\hat{P}$ (por ser un proyector) y $\hat{P}^\dagger=\hat{P}$ (por ser un operador hermítico).

b. Proyecta el estado $|\psi_1\rangle=\frac{2}{\sqrt{5}}|0\rangle+\frac{i}{\sqrt{5}}|1\rangle$ al subespacio generado por $|\psi_0\rangle$.

**Respuesta:**
**Inciso a:**
```python
#Escriba su código aquí para la parte a
from qiskit.quantum_info import Statevector, Operator
import numpy as np

zero_sv = Statevector.from_label("0")
zero_sv = (1j / np.sqrt(3)) * zero_sv
one_sv = Statevector.from_label("1")
one_sv = (np.sqrt(2/3)) * one_sv

full_sv = zero_sv + one_sv

# Con esto conseguimos el producto externo, o sea el Operador
resultado = np.outer(full_sv, full_sv.conjugate())

# Ahora usamos np.dot() para realizar la mutliplicación matricial
res = np.dot(resultado, resultado)

# Usamos np.allclose() para verificar que el resultado es igual al operador original
print("Cumple la condición de idempotencia?: ")
print(np.allclose(resultado, res))

print("Cumple la condición de ser igual a su traspuesta conjugada?: ")
print(np.allclose(resultado, resultado.conj().T))
```



**Inciso B:**
```python
#Escriba su código aquí para la parte b
from qiskit.quantum_info import Statevector, Operator
import numpy as np

zero_sv = Statevector.from_label("0")
zero_sv = (2 / np.sqrt(5)) * zero_sv

one_sv = Statevector.from_label("1")
one_sv = (1j / np.sqrt(5)) * one_sv

full_sv = zero_sv + one_sv

cero_sv = Statevector.from_label("0")
cero_sv = (1j / np.sqrt(3)) * cero_sv

uno_sv = Statevector.from_label("1")
uno_sv = (np.sqrt(2/3)) * uno_sv

completo = cero_sv + uno_sv

# Con esto conseguimos el producto externo, o sea el Operador
resultado = np.outer(completo, completo.conjugate())

res = resultado @ full_sv.data
print(res)
```
>Que dolor de cabeza de ejercicio lrpm


## Ejercicio 12: Condición de clausura
**La representación matricial del operador identidad es:**
$$
\begin{equation}
I=\begin{bmatrix}
1 & 0 & \cdots & 0 \\
0 & 1 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1
\end{bmatrix}
\end{equation}
$$

Dada una base [[ortonormal]] $\left\{|u_1\rangle,\ldots ,|u_n\rangle\right\}$, podemos expresar el operador identidad como:
$$
\begin{equation}
\hat{I}=\sum_{i=0}^n|u_i\rangle\langle u_i|
\end{equation}
$$
Esta expresión se denomina **condición de clausura**. Cada término de la suma puede pensarse como un proyector $\hat{P}_i$ (al subespacio generado por $|u_i\rangle$), en donde todos los proyectores son ortogonales entre sí: $\hat{P}_i\cdot\hat{P}_j=\vec{0}$ para $i\neq j$.

Teniendo esto en cuenta, obtén el operador identidad a partir de las bases $\left\{|0\rangle,|1\rangle\right\}$, $\left\{|+\rangle,|-\rangle\right\}$ y $\left\{|i\rangle,|-i\rangle\right\}$.

**Respuesta:**





