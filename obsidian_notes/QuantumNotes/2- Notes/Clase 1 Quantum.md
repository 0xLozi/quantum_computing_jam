Estos son los tres recursos que distinguen a una computadora cuántica de una clásica:

1. **Superposición**
Un bit clásico está en 0 o en 1. Un `qubit` puede estar en un estado que es una combinación lineal de ambos:
$$|\psi\rangle = \alpha|0\rangle + \beta|1\rangle, \qquad |\alpha|^2+|\beta|^2=1$$
Con $\alpha,\beta$ números complejos (las amplitudes). Mientras no lo midas, el `qubit` no tiene un valor definido de 0 o 1; al medirlo, colapsa a 0 con probabilidad $|\alpha|^2$ o a 1 con probabilidad $|\beta|^2$.

**La sutileza clave:** no es simplemente "0 y 1 a la vez". Si fuera solo eso, sería equivalente a una moneda clásica cargada (un bit probabilístico). La diferencia es que las amplitudes son complejas: tienen magnitud y fase. Esa fase es lo que permite que las amplitudes se sumen o se cancelen, es decir, que haya interferencia. Sin fase, la superposición no te da ninguna ventaja.

**Analogía:** una moneda girando en el aire. Mientras gira no es "cara" ni "cruz", pero cuando la atajás (medís) cae en una de las dos.


2. **Entrelazamiento**
Es una correlación entre `qubits` que no se puede describir `qubit` por `qubit`. El ejemplo canónico es el estado de Bell:
$$|\Phi^+\rangle = \frac{|00\rangle+|11\rangle}{\sqrt{2}}$$
Acá no podés decir "el `qubit` 1 está en tal estado y el `qubit` 2 en tal otro". El estado solo tiene sentido como par. Si medís el primero y sale 0, el segundo inmediatamente es 0; si sale 1, el segundo es 1. No importa la distancia.

¿No es lo mismo que dos cartas metidas en sobres (una roja, una negra) y llevarse uno cada uno? No: en el caso de las cartas, cada sobre ya tenía un color definido, solo que no lo sabías. En el estado entrelazado, ninguno de los dos tiene un valor definido hasta que medís uno. Esa diferencia se detecta experimentalmente: las correlaciones entrelazadas violan las desigualdades de Bell, algo imposible de reproducir con cualquier mecanismo clásico de "valores ocultos".
El entrelazamiento es el recurso detrás de la teleportación cuántica, la criptografía cuántica y buena parte de la ventaja de los algoritmos cuánticos.


3. **Interferencia**
Como las amplitudes son números complejos, se comportan como ondas: dos caminos que llevan al mismo resultado pueden sumarse (reforzarse) o restarse (cancelarse).
El ejemplo más limpio, con la compuerta de Hadamard (que vas a ver en el Ejercicio 10):
$$H|0\rangle = |+\rangle = \frac{|0\rangle+|1\rangle}{\sqrt2}, \qquad H|+\rangle = |0\rangle$$
Aplicar $H$ dos veces devuelve el estado original. ¿Por qué? Porque los dos caminos que llevan a |1⟩ llegan con amplitudes opuestas y se cancelan (interferencia destructiva), mientras que los que llevan a |0⟩ se refuerzan.
Esto es lo que hace útil a un algoritmo cuántico. Un algoritmo cuántico se diseña para que las amplitudes de los caminos que llevan a respuestas incorrectas se cancelen, y las de los caminos que llevan a la respuesta correcta se sumen. Así, al medir, obtenés la respuesta buena con alta probabilidad. Sin interferencia, medir un estado en superposición solo te daría resultados aleatorios, sin ventaja.


La definición de Wikipedia que encontraste está muy enfocada en **física óptica y comunicaciones**, no tanto en hardware de computación. Como vos venís del palo de la programación y los sistemas de bajo nivel, la mejor forma de entender esto es pensar en la integridad de la memoria y las señales físicas.
