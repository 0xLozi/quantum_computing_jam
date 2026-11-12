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

#### Me confundi
Puse el resultado del 14 en el 13... Bueno... Lo bueno es que van de la mano sinceramente: 
- Cuando el estado es un autovctor del operador, el valor esperado coincide exactametne con el autovalor (1)
- Cuando el estado está en una superposición equitativa respecto a ese operador, el resultado es 0

Bueno, en realidad esta sería la solución al 13, aunque igualmente no está mal la implementación de arriba... Pero en realidad esa debe ir en la 14

```python
# Escriba su solución aquí

ops = {
    "I": Operator.from_label("I"),
    "X": Operator.from_label("X"),
    "Y": Operator.from_label("Y"),
    "Z": Operator.from_label("Z"),
}

def desc_espectral(A: Operator):

    """Devuelve (A_espec, aval, avec) donde A_espec = sum_i λ_i |φ_i><φ_i|."""

    M = A.data

    # Obtengo autovalores y autovectores de la matriz M
    aval, avec = np.linalg.eig(M)

    # Reconstruyo el operador a partir de sus autovalores y autovectores
    A_espec = np.zeros_like(M, dtype=complex)
    for k in range(len(aval)):
        v = avec[:, k]
        # Normalizo:
        v = v / np.linalg.norm(v)
        # Armo la suma espectral
        A_espec += aval[k] * np.outer(v, v.conj())
    return A_espec, aval, avec

for name, A in ops.items():
    A_espec, aval, avec = desc_espectral(A)

    # Devuelve True si la descomposición espectral es equivalente a la matriz
    desc_espectral_ok = np.allclose(A_espec, A.data, atol=1e-12)
    print(f"\n=== {name} ===")
    print("Matriz original:\n", A.data)
    print("Autovalores:\n", aval)
    print("Autovectores:\n", avec)
    print("Reconstrucción espectral:\n", A_espec)
```

**Expliquemos un poco sobre este código:**
- `np.linalg.eig` es una función integrada de `NumPy` que está diseñada para calcular automáticamente los autovalores y autovectores de cualquier matriz numérica. Lo cual es un golazo para nosotros.

- `aval` y `avec`: Serían nombres de variables abreviados para mayor comodidad:
	- `aval`: **avalores** (los autovalores $\lambda$)
	- `avec`: **avectores** (o sea las columnas con los autovectores $\vert{}\phi\rangle$)

- `dtype=complex`: Sería la **mecánica cuántica**, los estado y operadores de manera frecuente usan números complejos. Dicha instrucción le avisa a python que reserve memoria para guardar números complejos (los cuales tienen parte real e imaginaria, tales como 0.+0.j), lo que hace que evite errores de formato o pérdida de datos.

- `np.zeros_like(M, dtype=complex)`: Es una función util de `NumPy` la cual crea una matriz llena de ceros la cual tiene exactamente la misma forma y dimensiones que otra matriz de referencia (que en este caso, la matriz M del operador).
  **Ejemplo:** Si $M$ es una matriz de $2 \times 2$, `np.zeros_like(M)` va a crear una matriz de $2 \times 2$ llena de ceros.


### Ejercicio 14: valores medios
El **valor medio** - también conocido como *media*, *valor esperado* o *esperanza* - de un observable $\mathcal{A}$ en un estado $|\psi\rangle$ está dado por:
$$
\begin{equation}
\langle\hat{A}\rangle=\langle\psi|\hat{A}|\psi\rangle
\end{equation}
$$
Obtén los valores esperados de los operadores de Pauli y la compuerta de Hadamard (ver los ejercicios 9 y 10) para los estados $|0\rangle$, $|1\rangle$, $|+\rangle$, $|-\rangle$, $|i\rangle$ y $|-i\rangle$.
