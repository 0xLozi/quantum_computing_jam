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
- `np.linalg.eig` es una función integrada de `NumPy` que está diseñada para calcular automáticamente los autovalores y autovectores de cualquier matriz numérica. Lo cual creo que nos va a servir bastante.

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

**Ahora si... El resultado en cuestión:**
```python
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

### Ejercicio 15: midiendo qubits
Aplica la compuerta de Hadamard, $\hat{H}$, al estado $|\psi_0\rangle=\frac{i}{\sqrt{3}}|0\rangle+\sqrt{\frac{2}{3}}|1\rangle$ y muestra en la terminal el estado $|\psi_0'\rangle=\hat{H}|\psi_0\rangle$ (este estado ya está normalizado, por lo que es un qubit válido). Observa el cambio de probabilidades de obtener cada estado de la base computacional al medir $|\psi_0'\rangle$.

#### Algo a tener en cuenta
La compuerta de Hadamard ($\hat{H}$), según lo que investigué, es una operación cuántica que actúa sobre un qubit. Su función principal sería la de actuar como un puente que convierte estados definidos en superposiciones y viceversa.

Funciona como la herramienta la cual genera los estados de la base de Hadamard:
- Si le aplico $\hat{H}$ al estado $\vert{}0\rangle$, el resultado es el estado de superposición $\vert{}+\rangle$
- Si le aplico al estado $\vert{}1\rangle$, obtengo $\vert{}-\rangle$.

Lo que hace es rotar el vector de estado en la esfera de Bloch, cambiando la perspectiva o la "base" desde la cual medimos el sistema.

```python
# Escriba su solución aquí
from qiskit.quantum_info import Statevector, Operator
import numpy as np

zeros_ones = Statevector([1j/np.sqrt(3), np.sqrt(2/3)])
print("Estas son las probabilidades antes de la aplicación de la compuerta {}", zeros_ones.probabilities_dict())

zeros_ones_H = zeros_ones.evolve(Operator.from_label("H"))
print("Probabilidades nuevas: {}", zeros_ones_H.probabilities_dict())
```

## Ejercicio 16: conservación de la norma
Los operadores unitarios $\hat{U}$ preservan la norma de los kets; es decir, se cumple que $||\hat{U}|\psi\rangle||=|||\psi\rangle||$.

Comprueba esta propiedad creando el operador unitario con la siguiente representación matricial (en la base computacional):
$$
\begin{equation}
U_1=\frac{1}{\sqrt2}\begin{bmatrix} 1 & i \\ i & 1 \end{bmatrix}
\end{equation}
$$
Luego, aplícalo al ket $|+\rangle$ y verifica que su norma siga siendo unitaria.

#### Respuesta:
**Algo para tener en cuenta es esto:** 
- El **operador identidad** es la matriz con unos en la **diagonal** y ceros en el resto. Por ende al aplicarla a un vector pues este lo deja igual.
- El adjunto $U^\dagger$ **es la transpuesta conjugada:** transpones la matriz y ademas conjugas cada número complejo (o sea cambia $i$ por $-i$). Con matrices reales coincide con la transpuesta, y con complejas pues no.

La ecuación $\hat{U}^\dagger\hat{U}=\hat{I}$ dice que **$U^\dagger$ deshace lo que hizo $U$**. Aplicar $U$ y luego $U^\dagger$ a un qubit es:
$$U^\dagger\big(U|\psi\rangle\big)=\big(U^\dagger U\big)|\psi\rangle=I|\psi\rangle=|\psi\rangle$$
Primero se aplica $U$, después $U^\dagger$, y por la definición de unitario el producto de ambas es la identidad, que no cambia nada.

**Resultado**
```python
#Escriba su solución aquí
from qiskit.quantum_info import Statevector, Operator
import numpy as np

M = np.array([[1, 1j],
              [1j, 1]])

molde = (1 / np.sqrt(2)) * M

U1 = Operator(molde)

if U1.is_unitary():
    print("Es unitario")
    plus_sv = Statevector.from_label("+")
    evolved = plus_sv.evolve(U1)
    # Ahora hay que calcular la norma!!!
    resultado = np.linalg.norm(evolved.data)
    print(resultado)
    print("Amplitudes de evolved")
    print(evolved)
    print("Amplitudes de |+>")
    print(plus_sv)
else: 
    print("No es unitario")
```

## Ejercicio 17: multiplicando operadores unitarios
El producto de dos operadores unitarios da como resultado otro operador unitario.

Comprueba esta propiedad creando el operador $\hat{U}_3=\hat{U}_1\cdot\hat{U}_2$, donde $\hat{U}_1$ es el operador del ejercicio anterior y $\hat{U}_2=\frac{\sqrt3}{2}|0\rangle\langle0|-\frac{1}{2}e^{-i\frac{\pi}{7}}|0\rangle\langle1|+\frac{1}{2}e^{i\frac{\pi}{5}}|1\rangle\langle0|+\frac{\sqrt3}{2}e^{i\left(\frac{\pi}{5}-\frac{\pi}{7}\right)}|1\rangle\langle1|$. Verifica que $\hat{U}_3^\dagger\hat{U}_3=\hat{I}$.

```python
#Escriba su solución aquí
import numpy as np

q2 = np.array([
    [np.sqrt(3)/2, -1/2 * (np.exp(-1j * np.pi/7))],
    [1/2 * (np.exp(1j * np.pi / 5)), np.sqrt(3)/2 * (np.exp(1j * ((np.pi / 5) - (np.pi / 7))))],
])

U2 = Operator(q2)
print("U2 unitario: ", U2.is_unitary())

U1 = Operator((1/np.sqrt(2)) * np.array([[1, 1j],
                                         [1j, 1]]))

U3 = U1.dot(U2)
print("U3 unitario: ", U3.is_unitary())
```

Acá utilicé un concepto el cual no sabia y que me sirvió mucho para saber cómo realizar la matriz sin morir en el intento: [[suma de productos externos]]


## Ejercicio 18: creando bases
Dada una [[base ortonormal]] $\left\{|u_1\rangle,\ldots,|u_n\rangle\right\}$, podemos obtener otra base ortonormal  $\left\{|w_1\rangle,\ldots,|w_n\rangle\right\}$ multiplicando cada vector por un operador unitario: $|w_i\rangle=\hat{U}|u_i\rangle$.

Obtén una base a partir la base computacional y el operador $\hat{U}_2$ del ejercicio anterior. Comprueba que la nueva base sea ortonormal.

**Respuesta**
```python
#Escriba su solución aquí
from qiskit.quantum_info import Statevector, Operator
import numpy as np

q2 = np.array([
    [np.sqrt(3)/2, -1/2 * (np.exp(-1j * np.pi/7))],
    [1/2 * (np.exp(1j * np.pi / 5)), np.sqrt(3)/2 * (np.exp(1j * ((np.pi / 5) - (np.pi / 7))))],
])

U2 = Operator(q2)


u0 = Statevector.from_label('0')
u1 = Statevector.from_label('1')

w0 = u0.evolve(U2)
w1 = u1.evolve(U2)

print(w0.inner(w0))
print(w1.inner(w1))
print(w0.inner(w1))
print(w1.inner(w0))
```

**Lo que salió en pantalla:**
```plaintext
(0.9999999999999999+0j)
(1+0j)
(-6.938893903907228e-18-5.551115123125783e-17j)
(-6.938893903907228e-18+5.551115123125783e-17j)
```

**Explicación del resultado:**
- `(1+0j)` y `(0.9999999999999999+0j)` son un solo número complejo, con parte real y parte imaginaria. Vale 1; lo cual confirma que $\langle w_0 \vert w_0 \rangle$ = $\langle w_1 \vert w_1 \rangle = 1$: Los vectores son normales.
- **`e-18` y `e-17`** significan $\times10^{-18}$ y $\times10^{-17}$. Entonces `-6.9e-18-5.5e-17j` es un número de parte real $\approx-0.000000000000000007$ y parte imaginaria $\approx-0.00000000000000006$. En la práctica, **cero**. Ese residuo es el redondeo del que hablamos. Esto confirma que los $|w_i\rangle$ son **ortogonales**.

> [!Tip]
> **Nota de color:**
> Los dos productos cruzados son casi iguales y solo cambia el signo de la parte imaginaria. Eso es un conjugado completo, y es la propiedad que aparece en el ejercicio 4 (simetría conjugada):
> $$
> \large{\langle w_1|w_0\rangle=\langle w_0|w_1\rangle^*}
> $$
> > Si el producto interno fuera exactamente 0, pues no se notaría. Acá justamente se nota debido a que aparece el residuo.
> 
> **Conclusión del ejercicio 18**
> **Los $|w_i\rangle=U_2|u_i\rangle$ forman una base ortonormal:** miden 1 y son ortogonales entre sí. Es lo que predice la teoría, porque $U_2$ es unitario y conserva los productos internos.


## Ejercicio 19
Comprueba que los operadores $X$, $Y$, $Z$ y $H$ (ver ejercicios 9 y 10) son unitarios. Investiga sobre los operadores $S$ y $T$, observa el efecto que tienen sobre los estados de la base computacional y verifica que sean [[Operador unitario|operadores unitarios]].

```python
# Escriba su solución aquí
from qiskit.quantum_info import Statevector, Operator
import numpy as np

X = Operator.from_label("X")
Y = Operator.from_label("Y")
Z = Operator.from_label("Z")
H = Operator.from_label("H")

print("Es unitario X?: ", X.is_unitary())
print("Es unitario Y?: ", Y.is_unitary())
print("Es unitario Z?: ", Z.is_unitary())
print("Es unitario H?: ", H.is_unitary())

# Ahora me pide sobre investigar los operadores S y T
S = Operator.from_label("S")
T = Operator.from_label("T")

# Existen y no me tira error, ahora apliquemoslos a la base computacional
zero_sv = Statevector.from_label("0")
one_sv = Statevector.from_label("1")

# Aplicamos S
zero_S = zero_sv.evolve(S)
one_S = one_sv.evolve(S)

# Aplicamos T
zero_T = zero_sv.evolve(T)
one_T = one_sv.evolve(T)

# Para confirmar que son unitarios tenemos dos formas: Usar .is_unitary() dentro de los operadores. O ver que, tras aplicarle el adjunto al operador multiplico por el ket pues este mismo devuelva el ket original
print("Comparación entre zero_sv y zero_S")
print(f"zero_sv: {zero_sv}    \nzero_S: {zero_S}")

print("\n")
print("Comparación entre one_S y one_sv")
print(f"one_sv: {one_sv}\n one_S: {one_S}")

print("\n")

print("Comparación entre zero_sv y zero_T")
print(f"zero_sv: {zero_sv}    \nzero_S: {zero_T}")

print("\n")
print("Comparación entre one_sv y one_T")
print(f"one_sv: {one_sv}\n one_S: {one_T}")
```

**Salida:**
```shell
Es unitario X?:  True
Es unitario Y?:  True
Es unitario Z?:  True
Es unitario H?:  True
Comparación entre zero_sv y zero_S
zero_sv: Statevector([1.+0.j, 0.+0.j],
            dims=(2,))    
zero_S: Statevector([1.+0.j, 0.+0.j],
            dims=(2,))


Comparación entre one_S y one_sv
one_sv: Statevector([0.+0.j, 1.+0.j],
            dims=(2,))
 one_S: Statevector([0.+0.j, 0.+1.j],
            dims=(2,))


Comparación entre zero_sv y zero_T
zero_sv: Statevector([1.+0.j, 0.+0.j],
            dims=(2,))    
zero_S: Statevector([1.+0.j, 0.+0.j],
            dims=(2,))


Comparación entre one_sv y one_T
one_sv: Statevector([0.+0.j, 1.+0.j],
            dims=(2,))
 one_S: Statevector([0.        +0.j        , 0.70710678+0.70710678j],
            dims=(2,))
```
### Anotaciones importantes relacionadas a la salida
**Cómo leer `0.+1.j` y `0.70710678+0.70710678j`**
Cada uno es un sólo número complejo, con la forma de `parte_real + parte_imaginaria j` :

- `0.+1.j` es $0+1\cdot i=i$.
- `0.70710678+0.70710678j` es $0.7071+0.7071\,i$. No es gigante: cada parte es $\frac{1}{\sqrt2}$, un número que ya he viste varias veces.

Un `Statevector` de un qubit imprime **dos** números complejos, uno por amplitud: primero la de $|0\rangle$ y después la de $|1\rangle$.

**Lo que he visto, ordenado**
- **Con $|0\rangle$:** tanto $S$ como $T$ lo dejan igual.
- **Con $|1\rangle$:** la primera amplitud sigue en 0, como antes. Lo que cambió es la **segunda**: en $S$ pasó de $1$ a $i$, y en $T$ pasó de $1$ a $0.7071+0.7071i$.

**En síntesis**
- **No “separa” nada.** $S$ y $T$ multiplican la amplitud de $|1\rangle$ por un número complejo de módulo 1: $i$ para $S$ y $e^{i\pi/4}$ para $T$. La amplitud de $|0\rangle$ queda intacta. Ese número solo la gira en el plano complejo, sin cambiar su tamaño.
- **La probabilidad** es el módulo al cuadrado de **la amplitud completa**: $a^2+b^2$ para $a+bi$. Dar 0.5 y 0.5 en cada parte y sumarlas da 1, y eso es lo que importa.


### Ejercicio 20: fase global y relativa
a. Crea los estados $|\psi_0\rangle=\frac{i}{\sqrt{3}}|0\rangle+\sqrt{\frac{2}{3}}|1\rangle$ y $|\psi_3\rangle=e^{i\frac{3\pi}{4}}|\psi_0\rangle$. Luego, aplica la compuerta de Hadamard a ambos estados y comprueba que las probabilidades de obtener $0$ ó $1$ al medir son las mismas para ambos qubits; es decir, son físicamente indistinguibles.

```python
#Escriba su código aquí para la parte a
from qiskit.quantum_info import Statevector, Operator
import numpy as np

zero_sv = (1j / np.sqrt(3)) * Statevector.from_label("0")
one_sv = (np.sqrt(2/3)) * Statevector.from_label("1")

w0 = zero_sv + one_sv
w3 = np.exp(1j * ((3 * np.pi) / 4)) * w0

H = Operator.from_label("H")

w0_H = w0.evolve(H)
w3_H = w3.evolve(H)

print("Probabilidades para w0")
resultado = w0.probabilities_dict()
print(resultado)
print("Tras apicar Hadamard: ")
print(w0_H.probabilities_dict())

print("\n")

print("Probabilidades para w3")
resultado = w3.probabilities_dict()
print(resultado)
print("Tras apicar Hadamard: ")
print(w3_H.probabilities_dict())
```

**Respuesta:**
```shell
Probabilidades para w0
{np.str_('0'): np.float64(0.3333333333333334), np.str_('1'): np.float64(0.6666666666666666)}
Tras apicar Hadamard: 
{np.str_('0'): np.float64(0.4999999999999999), np.str_('1'): np.float64(0.4999999999999999)}


Probabilidades para w3
{np.str_('0'): np.float64(0.3333333333333334), np.str_('1'): np.float64(0.6666666666666666)}
Tras apicar Hadamard: 
{np.str_('0'): np.float64(0.5000000000000001), np.str_('1'): np.float64(0.5000000000000001)}
```
Las probabilidades `w0` y `w3` coinciden antes de $H$ ($1/3$ y $2/3$) y después de $H$ ($50/50$, salvo redondeo). Esto comprueba que los dos estados son físicamente indistinguibles. El `np.str_(...)` es solo cómo imprime esta versión de `numpy` las claves `'0'` y `'1'`. 

**Por qué dio 50/50**
En la esfera de Bloch, $H$ **intercambia el eje $x$ con el eje $z$** (y invierte $y$). el estado $|\psi_0\rangle$ tiene una amplitud imaginaria y otra real, o sea una fase relativa de $-90^\circ$, así que está en el lado del eje $y$ y su coordenada $x$ vale $0$. Después de $H$, la nueva coordenada $z$ es la vieja $x$, o sea $0$. Con $z=0$ el estado queda en el ecuador, que es 50/50.

Fue una coincidencia de la fase que tiene ese estado en particular. Lo mismo vale para $|\psi_3\rangle$, porque la fase global no mueve el punto en la esfera.

#### Inciso b
b. Crea un nuevo estado a partir del qubit $|\psi_0\rangle$, agregándole una fase relativa de $45^\circ$ al estado $|1\rangle$ (esta fase puede expresarse como $e^{i\pi/4}$). Luego, aplica la compuerta de Hadamard a ambos estados y comprueba que las probabilidades de obtener $0$ ó $1$ al medir no son las mismas para ambos qubits.

Ya encontré la confusión que tenia: El angulo no es algo extra que se agrega a una amplitud, sino que es una propiedad del propio número complejo.
**Me explico:** Cada **número complejo ya tiene su ángulo**
La amplitud $\beta=\sqrt{\frac23}$ es un número real positivo. En el plano complejo es una flecha que apunta hacia la derecha, así que su ángulo es $0^\circ$, sin que nadie se lo haya puesto. Al escribir el número, ya quedó fijado:

|amplitud|ángulo|
|---|---|
|$\sqrt{\frac23}$ (real positivo)|$0^\circ$|
|$i/\sqrt3$|$90^\circ$|
|$-\sqrt{\frac23}$ (real negativo)|$180^\circ$|
### Ejercicio 21
```python
#Escriba su código aquí

plot_bloch_multivector(Statevector.from_label('+-rl01'), reverse_bits = True)
```

**Conexión con la teoría**
- Los estados de los ejes $x$ e $y$ están sobre el **ecuador**, y por eso dan 50/50 al medir en la base computacional.
- Los estados de los polos dan resultado seguro: 100% de medir 0 en el norte y 100% de medir 1 en el sur.
- Los pares $|+\rangle,|-\rangle$ y $|i\rangle,|-i\rangle$ difieren solo en la **fase relativa**: $|+\rangle\to|-\rangle$ es un giro de $180^\circ$ alrededor del eje $z$, y $|+\rangle\to|i\rangle$ es un giro de $90^\circ$. Esa es la rotación que hace la compuerta $S$, que dejamos pendiente en el ejercicio 19.

### Ejercicio 22: Operadores unitarios en la esfera de Bloch
Aplica dos veces la compuerta de Hadamard al qubit $|0\rangle$ y observa su evolución temporal representando cada nuevo estado en la esfera de Bloch.