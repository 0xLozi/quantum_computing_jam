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
**Caso: $\left\{|0\rangle,|1\rangle\right\}$** 
```python
# Escriba su solución aquí
from qiskit.quantum_info import Statevector, Operator
import numpy as np

zero_sv = Statevector.from_label("0")
one_sv = Statevector.from_label("1")

pe_zero = np.outer(zero_sv, zero_sv.conjugate())
pe_one = np.outer(one_sv, one_sv.conjugate())

resultado = pe_zero + pe_one

print(resultado)
```
Con esto, vemos que efectivamente el resultado es la matriz de identidad!!!!

```plaintext
[[1.+0.j 0.+0.j]
 [0.+0.j 1.+0.j]]
```

Con el resto:
```python
# Escriba su solución aquí
from qiskit.quantum_info import Statevector, Operator
import numpy as np

zero_sv = Statevector.from_label("0")
one_sv = Statevector.from_label("1")

pe_zero = np.outer(zero_sv, zero_sv.conjugate())
pe_one = np.outer(one_sv, one_sv.conjugate())

resultado = pe_zero + pe_one

print("Caso sumando proyector de zero y one")
print(resultado)

print("Caso plus y minus")
plus_sv = Statevector.from_label("+")
minus_sv = Statevector.from_label("-")

pe_plus = np.outer(plus_sv, plus_sv.conjugate())
pe_minus = np.outer(minus_sv, minus_sv.conjugate())

resultado = pe_plus + pe_minus
print(resultado)

print ("Caso i y -i")
r_sv = Statevector.from_label("r")
l_sv = Statevector.from_label("l")

pe_r = np.outer(r_sv, r_sv.conjugate())
pe_l = np.outer(l_sv, l_sv.conjugate())

resultado = pe_r + pe_l
print(resultado)
```

**Resultado:** 
```plaintext
Caso sumando proyector de zero y one
[[1.+0.j 0.+0.j]
 [0.+0.j 1.+0.j]]
Caso plus y minus
[[1.+0.j 0.+0.j]
 [0.+0.j 1.+0.j]]
Caso i y -i
[[1.+0.j 0.+0.j]
 [0.+0.j 1.+0.j]]
```



## Ejercicio 13: teorema de descomposición cerebral
Podemos obtener los autovalores $\lambda_i$ y sus autovectores asociados $|\phi_i\rangle$ de un operador lineal $\hat{A}$. Cada par cumplirá con la ecuación $\hat{A}|\phi_i\rangle=\lambda_i|\phi_i\rangle$.

El **teorema de descomposición espectral** dice que podemos expresar al operador lineal de la siguiente manera:

$$
\begin{equation}
\hat{A}=\sum_i\lambda_i|\phi_i\rangle\langle\phi_i|
\end{equation}
$$

Verifica que se cumple el teorema de descomposición espectral para los operadores de Pauli (ver el ejercicio 9), obteniendo los autovectores y autovalores de sus representaciones matriciales.

Por lo que vi ahora es verdadero para el caso de X:
```python
# Escriba su solución aquí
from qiskit.quantum_info import Operator
from qiskit.quantum_info import Statevector

X = Operator.from_label('X').data

minus_sv = Statevector.from_label("-")
pe_minus = np.outer(minus_sv, minus_sv.conjugate())

plus_sv = Statevector.from_label("+")
pe_plus = np.outer(plus_sv, plus_sv.conjugate())

expected = 1 * (pe_plus) + (-1) * (pe_minus)

print("Usando teorema de descomposición espectral")
print(expected)

print("Operador original")
print(X)
```
**Resultado:**
```plaintext
Usando teorema de descomposición espectral
[[0.+0.j 1.+0.j]
 [1.+0.j 0.+0.j]]
Operador original
[[0.+0.j 1.+0.j]
 [1.+0.j 0.+0.j]]
```

Efectivamente, dan lo mismo. Sin embargo tengo que hacer esto para todos los operadores restantes... Así que opté por revisar la solución y ver la opción mas eficiente, la cual es esta:
```python
# Escriba su solución aquí
I = Operator.from_label('I')
X = Operator.from_label('X')
Y = Operator.from_label('Y')
Z = Operator.from_label('Z')

q0 = Statevector.from_label("0")
q1 = Statevector.from_label("1")

q_plus = Statevector.from_label("+")
q_minus = Statevector.from_label("-")
right_sv = Statevector.from_label('r')
left_sv = Statevector.from_label('l')


ops = {
    "I": I,
    "X": X,
    "Y": Y,
    "Z": Z,
}

svs = {
    "0": q0,
    "1": q1,
    "+": q_plus,
    "-": q_minus,
    "i": right_sv,
    "-i": left_sv,
}

for sv_name, sv in svs.items():
    print(f"Estado {sv_name}:")
    for op_name, op in ops.items():
        ev = sv.expectation_value(op)
        print(f"⟨{op_name}⟩ = {ev}")
    print("\n")
```

[[expectation value|Explicación sobre expectation_value en StateVector]]