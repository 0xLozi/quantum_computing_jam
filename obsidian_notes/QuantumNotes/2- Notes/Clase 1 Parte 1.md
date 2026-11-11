El objetivo de esta clase es familizarizarse con las herramientas matemáticas que se utilizan en la computación cuántica (y en la mecánica cuántica en general).  

Estas herramientas probablemente ya sean conocidas por muchos, ya que se tratan de temas fundamentales del álgebra lineal: vectores fila, vectores columna, matrices, productos internos, etc. También es necesario tener algún conocimiento de números complejos, y puede ser útil tener conocimientos de probabilidad. Para los que necesiten un repaso rápido de estos temas, pueden consultar la bibliografía complementaria al final de esta clase.

Antes de adentrarnos en los aspectos más técnicos, presentamos una breve introducción conceptual e histórica de la computación cuántica. Quienes deseen saltearse esta parte (o volver a ella después), pueden seguir con las siguientes secciones.

## ¿Qué es la computación cuántica?

La **computación cuántica** es una manera de procesar información utilizando las reglas de la **mecánica cuántica** - como la *superposición*, el *entrelazamiento* y la *interferencia* - para resolver problemas que son muy difíciles para una computadora clásica (se cree que algunos de estos problemas son imposibles de resolver en máquinas clásicas).

En la práctica, una computadora cuántica no puede resolver cualquier problema más rápido que una computadora clásica, sino que ofrece ventajas específicas para ciertas familias de problemas como la *simulación* de sistemas cuánticos y *química cuántica*, algunas formas de *optimización* y *muestreo*, y tareas específicas de *criptografía*.

## ¿Cómo empezó todo esto?
<img src="images/feynman.jpg" align="right" width="200" />
La historia de la mecánica cuántica se remonta a más de 100 años atrás, pero la historia moderna de la computación cuántica comienza en la década de 1980 con el físico **Richard Feynman** (ver imagen a la derecha), quien argumentó que la manera correcta de simular sistemas cuánticos era a través de máquinas cuánticas, ya que la Naturaleza "no es clásica".

En los años posteriores, muchas personas comenzaron a interesarse en el desarrollo de este nuevo campo de estudio. En la década de 1990, el matemático **Peter Shor** descubrió un algoritmo cuántico de factorización en tiempo polinomial (exponencialmente más rápido que el mejor algoritmo clásico). Poco tiempo después, el informático **Lov Grover** halló una mejora importante respecto a los algoritmos clásicos para búsquedas en una secuencia no estructurada de datos. Estos dos algoritmos demostraron las potenciales ventajas de las computadoras cuánticas.

En la misma época, el físico **David DiVicenzo** estableció los criterios necesarios para construir computadoras cuánticas, lo que guió las investigaciones para las implementaciones de hardware en diferentes tecnologías.

Estos hitos (y otros que dejamos de lado en esta breve narración) definieron los inicios de la computación cuántica y guiaron su desarrollo hacia el panorama actual.

## ¿Dónde estamos ahora?

Los dispositivos que tenemos hoy en día todavía son demasiado [ruidosos](https://es.wikipedia.org/wiki/Ruido_cu%C3%A1ntico) y de escala intermedia ([NISQ](https://es.wikipedia.org/wiki/Cu%C3%A1ntica_de_escala_intermedia_ruidosa)), pero son útiles para el aprendizaje y la implementación de algunos algoritmos en hardware real. Las dos motivaciones principales para mejorar el estado actual de las computadoras cuánticas son:

- Obtener mejores *simulaciones* de materiales y compuestos químicos y biológicos para el desarrollo científico e industrial, que reproduzcan el comportamiento cuántico de estos sistemas y superen las limitaciones de las simulaciones clásicas actuales.

- Crear sistemas de *seguridad* y *criptografía* más seguros. La obtención de una computadora de muchos qubits tolerante a fallos podrá implementar exitosamente el algoritmo de Shor, volviendo obsoleta la *criptografía de clave pública RSA*.

Con mejores compiladores y técnicas de mitigación de errores, podemos extraer buenos resultados para problemas más pequeños antes de que la corrección de errores pueda ser implementada. Por eso, las empresas de tecnología, los laboratorios y los gobiernos están invirtiendo ahora: para construir el hardware, el software y la reserva de talentos. Así, cuando los sistemas escalen tendremos los problemas relevantes y las personas capacitadas para resolverlos.

En este contexto, **Qiskit** es un SDK (kit de desarrollo de software) creado por IBM para implementar circuitos y algoritmos cuánticos. Ofrece la posibilidad de correr programas en las computadoras cuánticas reales de IBM a través de sus servicios en la nube y utilizando *Qiksit Runtime*. También permite simular programas localmente.

## Vectores columna (kets)
<img src="images/paul-dirac.jpg" align="right" width="200" />Para expresar objetos en computación cuántica (y en mecánica cuántica en general), usamos la *notación de Dirac*, o *notación bra-ket*, la cual es más compacta para expresar elementos del álgebra lineal como vectores, matrices y productos internos. Fue creada en 1939 por el físico Paul Dirac (ver imagen a la derecha).

En la notación de Dirac, un **ket** $\left|v\right>$ representa un vector columna en un espacio vectorial complejo de $n$ dimensiones:

$$
\begin{equation}
\left|v\right>=\begin{bmatrix} z_1 \\ \vdots \\ z_n \end{bmatrix}\in\mathbb{C}^n
\end{equation}
$$

Los vectores columna - o kets - son lineales:

$$
\begin{equation}
\left|\alpha v_1+\beta v_2\right>=\alpha\left|v_1\right>+\beta\left|v_2\right>
\end{equation}
$$
## Qubits y superposición
En computación clásica, el **bit** es la unidad de información básica. Un bit puede tener uno de dos estados: $0$ ó $1$. Estos estados pueden representarse físicamente en una computadora con señales eléctricas; por ejemplo, con 0 y 5 volts respectivamente.

En computación cuántica, trabajamos con **qubits** (”bits cuánticos”). Al igual que un bit, un qubit tiene dos estados posibles: el estado $\left|0\right>$ y el estado $\left|1\right>$. Veremos que la manera en la que funcionan estos estados es bastante diferente a la manera en la que funcionan los estados clásicos.

$\left|0\right>$ y $\left|1\right>$ son estados especiales conocidos como estados de la **base computacional**, y forman una base ortonormal para este espacio vectorial. Podemos pensarlos a ambos como los siguientes vectores unitarios:

$$
\begin{equation}
\left|0\right>=\begin{bmatrix} 1\\ 0\end{bmatrix}
\qquad\qquad
\left|1\right>=\begin{bmatrix} 0\\ 1\end{bmatrix}
\end{equation}
$$

La diferencia entre un bit y un qubit es que un qubit puede encontrarse en otros estados que no son $\left|0\right>$ ó $\left|1\right>$, sino "*ambos a la vez*".
Este principio se llama **superposición**, y se representa mediante una *combinación lineal* de estados:

$$
\begin{equation}
\left|\psi\right>=\alpha\left|0\right>+\beta\left|1\right>=\alpha\begin{bmatrix} 1\\ 0\end{bmatrix}+\beta\begin{bmatrix} 0\\ 1\end{bmatrix}=\begin{bmatrix}\alpha \\ \beta\end{bmatrix}
\end{equation}
$$

donde $\alpha$ y $\beta$ son números complejos que suelen denominarse **amplitudes de probabilidad**.

Físicamente, podemos implementar un qubit con cualquier sistema cuántico que admita dos estados superpuestos: la polarización horizontal y vertical de un [fotón](https://es.wikipedia.org/wiki/Fot%C3%B3n), la orientación del [spin](https://es.wikipedia.org/wiki/Esp%C3%ADn) nuclear en un campo magnético uniforme, o dos estados diferentes de un [electrón orbitando un átomo](https://es.wikipedia.org/wiki/Modelo_at%C3%B3mico_de_Bohr#:~:text=El%20modelo%20at%C3%B3mico%20de%20Bohr,ocupar%20ciertos%20niveles%20de%20energ%C3%ADa.) (un estado base y un estado excitado).Sin embargo, por ahora los trataremos exclusivamente como objetos matemáticos. Esto nos permite construir una teoría de la información cuántica sin depender de su implementación física.

---

Cuando examinamos (observamos, medimos) un qubit, la superposición de estados *colapsa*. Por lo tanto, solo podemos medir uno de los dos estados clásicos representados por los estados de la base computacional:

- $0$, con una probabilidad $|\alpha|^2$;
- $1$, con una probabilidad $|\beta| ^2$.

Como la suma de probabilidades debe ser uno:

$$
\begin{equation}
|\alpha|^2+|\beta|^2=1
\end{equation}
$$

<img src="images/schrodinger-cat.avif" align="right" width="200" />Este comportamiento puede resultar extraño y contraintuitivo, ya que no se corresonde con ningún efecto que estemos acostumbrados a presenciar en nuestro mundo clásico y macroscópico. El físico Erwin Schrödinger demostró lo paradójico que puede resultar este comportamiento en su [famoso experimento mental](https://es.wikipedia.org/wiki/Gato_de_Schr%C3%B6dinger), en donde un gato encerrado en una caja con veneno está en una superposición de los estados "vivo" y "muerto" antes de que lo observemos.

¿Por qué ocurre este colapso de la superposición? Nadie sabe: es uno de los postulados fundamentales de la mecánica cuántica.

Geométricamente, que la suma de probabilidades sea igual a uno es equivalente a que el estado de un qubit esté *normalizado* (tenga longitud 1): es un *vector unitario* en un *espacio vectorial* complejo bidimensional ($\mathbb{C}^2$). Este espacio vectorial en el que "vive" el qubit se llama *espacio de Hilbert*: para los espacios vectoriales complejos y finitos con los que trataremos nosotros, es equivalente a hablar de un espacio vectorial con producto interno (el cual definiremos más adelante). Todos los sistemas físicos aislados de la mecánica cuántica se representan con un vector columna o ket de estado normalizado en un espacio de Hilbert.

**Nota 1:** la letra griega $\psi$ (*psi*) se utiliza para representar sistemas u objetos cuánticos en la mecánica cuántica ondulatoria. Representa la *función de onda* de ese objeto. Otros nombres comunes para funciones de onda son $\phi$  /$\varphi$ (*phi*) y $\pi$ (*pi*).

**Nota 2:** la función de onda de un sistema $\psi(\mathbf{x}, t)$ no tiene una interpretación física inmediata y no se puede observar. Sin embargo, es útil para construir una función de densidad de probabilidad dada por $\left|\psi(\mathbf{x},t)\right|^2=\psi^*(\mathbf{x},t)\psi(\mathbf{x},t)$ (por ser un número complejo), la cual nos permite conocer la probabilidad de encontrar al objeto cuántico en la posición $\mathbf{x}$ para un determinado tiempo $t$.

## Representando qubits en Qiskit
Podemos representar qubits con la clase ```Statevector```. Esta clase es útil para representar qubits individuales y múltiples qubits en conjunto (lo veremos más adelante).

Existen múltiples maneras de crear objetos de la clase ```Statevector```.
Dos formas importantes son el método ```.from_label('string')```, en donde ingresaremos un string representativo del estado que queremos crear, y utilizando un arreglo ```Statevector([α,β])```. Más adelante veremos cómo extraer el vector de estado de un circuito cuántico.

También existen múltiples maneras de visualizar la información del vector de estados; por ejemplo, imprimiendo con ```print()``` o con los métodos ```.data```, ```.draw()``` y ```.probabilities_dict()```.

Los siguientes ejercicios sirven para aplicar los conceptos básicos de esta clase. Los animamos a que consulten [la documentación apropiada](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.quantum_info.Statevector), sean curiosos, prueben distintos códigos para ver los resultados, investiguen lo que no entiendan y consulten a nuestros mentores por nuestro canal de Discord.


# Ejercicios
## Ejercicio 1
**a.** Crea el estado $\frac{1+i}{2} |0 \rangle + 1\frac{1-i}{2}|1\rangle$ utilizando un arreglo de **Python** para las amplitudes.
**b.** Muestra el estado en la terminal.
**c.** Muestra las probabilidades de medir 0 ó 1 en la terminal.
### Mis análisis
**Primero que nada tengo que recordar qué es un estado:**

El **bit** es la unidad de información básica, y un bit puede tener `1` o `2` estados (`0, 1`). 
En computación cuántica, nosotros trabajamos con **qubits** ("Bits cuánticos"). 

El **qubit** tiene dos estados posibles: 
- Estado $|0\rangle$
- Estado $|1\rangle$

**Podemos pensarlos como vectores unitarios:**
$$
\begin{equation}
\left|0\right>=\begin{bmatrix} 1\\ 0\end{bmatrix}
\qquad\qquad
\left|1\right>=\begin{bmatrix} 0\\ 1\end{bmatrix}
\end{equation}
$$
Un **qubit** puede encontrarse en ambos a la vez **"superposicíon"**:
$$
\begin{equation}
\left|\psi\right>=\alpha\left|0\right>+\beta\left|1\right>=\alpha\begin{bmatrix} 1\\ 0\end{bmatrix}+\beta\begin{bmatrix} 0\\ 1\end{bmatrix}=\begin{bmatrix}\alpha \\ \beta\end{bmatrix}
\end{equation}
$$
En donde $\alpha$ y $\beta$ son números complejos que suelen denominarse [[amplitudes de probabilidad]].

Para empaquetar las dos amplitudes de nuestro estado cuántico $\frac{1+i}{2} | 0 \rangle + \frac{1-i}{2}|1\rangle$, creamos una lista que contenga ambos valores en orden:
```python
amplitudes = [(1 + 1j) / 2, (1 - 1j) / 2]
```

**Puntos a tener en cuenta:**
- **Orden posicional:** El primer elemento de la lista corresponde siempre a la amplitud del estado $|0\rangle$ y el segundo a la del estado $|1\rangle$.
- **Sintaxis Compleja:** En Python, los números complejos vienen integrados de forma nativa; la unidad imaginaria `i` se denota escribiendo `1j` (o `j` después de un número)
### Resultado
#### Punto a
```python
#Escriba su código aquí para la parte a
from qiskit.quantum_info import Statevector

a = (1 + 1j) / 2
b = (1 - 1j) / 2
amplitudes = [a, b]
state = Statevector(amplitudes)
```
#### Punto b
```python
#Escriba su código aquí para la parte b
print(state)
```
#### Punto c
```python
#Escriba su código aquí para la parte c
state.probabilities_dict()
```


---
## Ejercicio 2: Algunos qubits famosos
Los siguientes estados son *muy* utilizados en computación cuántica:

$$
\begin{equation}
\left|+\right>=\frac{\left|0\right>+\left|1\right>}{\sqrt{2}}
\qquad\qquad
\left|-\right>=\frac{\left|0\right>-\left|1\right>}{\sqrt{2}}
\qquad\qquad
\left|i\right>=\frac{\left|0\right>+i\left|1\right>}{\sqrt{2}}
\qquad\qquad
\left|-i\right>=\frac{\left|0\right>-i\left|1\right>}{\sqrt{2}}
\end{equation}
$$

**a.** Crea los cuatro estados anteriores utilizando el método ```.from_label()``` y muéstralos en la terminal (investiga qué strings están permitidos para este método).

**b.** Muestra las probabilidades de obtener $0$ ó $1$ - $P(0)$ y $P(1)$ - para cada uno de los cuatro estados anteriores.

**c.** Crea el estado $\frac{\sqrt{3}}{2}\left|+\right>+\frac{1}{2}\left|-\right>$ y verifica que es un estado normalizado: $P(0)+P(1)=1$.

### Respuesta
Me piden que cree los cuatro estados disponibles usando el método `.from_label()` y los muestre en la terminal. Primero investiguemos para qué sirve el `.from_label()`.

Es simplemente un acceso directo útil **para construir estados cuánticos conocidos a partir de cadenas de texto** (strings)

Para los estados del Ejercicio 2, estos son los strings permitidos en `Statevector.from_label()`:

**`'0'` y `'1'` (Base computacional):** Son los estados definitivos o "clásicos".
- `'0'` equivale al vector $\begin{bmatrix} 1 \\ 0 \end{bmatrix}$ (100% de probabilidad de medir $0$)

**`'+'` y `'-'` (Superposición equitativa):** Son mezclas donde el $0$ y el $1$, tienen exactamente la misma [[magnitud]], pero cambia la fase relativa (el signo central).
- `'+'` es $\frac{\vert{}0\rangle + \vert{}1\rangle}{\sqrt{2}}$ (fase positiva).
- `'-'` es $\frac{\vert{}0\rangle - \vert{}1\rangle}{\sqrt{2}}$ (fase negativa, clave para la [[interferencia cuántica]]).

**`'r'` y `'l'` (Estados circulares):** Involucran la unidad imaginaria $i$ en las amplitudes, representando fases complejas.
- `'r'` (right) es $\frac{\vert{}0\rangle + i\vert{}1\rangle}{\sqrt{2}}$ (corresponde a $\vert{}i\rangle$).
- `'l'` (left) es $\frac{\vert{}0\rangle - i\vert{}1\rangle}{\sqrt{2}}$ (corresponde a $\vert{}-i\rangle$).

Todos estos estados describen a un único qubit, pero representan diferentes "orientaciones" o combinaciones de magnitud y fase en el espacio cuántico.

Investigando dentro de la documentación, me encontré que los strings permitidos para la función `.from_label()` son:
![[Pasted image 20261109143957.png]]

- Realizado ejercicio a!!!

Ahora me pide mostrar las probabilidades de obtener `0` ó `1`, lo cual sería $P(0)$ y $P(1)$ respectivamente, pero para cada uno de los cuatro estados anteriores.

Para eso almaceno en una variable el resultado de la llamada a cada uno de los `StateVector` sus probabilidades: `probabilities_dict()`.

Y con eso recibo un `dict`, el cual puedo acceder llamando al valor que estoy esperando. En este caso sería "0" y "1", y con eso anoto las probabilidades de cada uno:
```shell
Probs para 0.4999999999999999 0.4999999999999999 Probs para - 0.4999999999999999 0.4999999999999999 Probs para r 0.4999999999999999 0.4999999999999999 Probs para l 0.4999999999999999 0.4999999999999999
```


**Siguiente ejercicio: punto c**
**c.** Crea el estado $\frac{\sqrt{3}}{2}\left|+\right>+\frac{1}{2}\left|-\right>$ y verifica que es un estado normalizado: $P(0)+P(1)=1$.

**Algo que hay que tener en cuenta:**
Cuando el enunciado menciona de que $P(0) + P(1) = 1$, esto significa que la suma total de las probabilidades de todos los resultados posibles debe ser igual a 1 (100%).
- **Amplitudes y Vectores:** Los números como $\frac{\sqrt{3}}{2}$ o $\frac{1}{2}$ Actúan como pesos o factores de escala que determinan qué tanto influye cada estado base (o en este caso, cada sub-estado como $|+\rangle$ y $|-\rangle$) en la superposición total.

- **El rol de la raíz:** El denominador $\sqrt{2}$ que aparece en $|+\rangle$ y $|-\rangle$ es el factor geométrico que asegura que el vector tenga longitud 1.

Según lo que investigué, los objetos Statevector en Qiskit admiten operaciones algebráicas directas. Entonces cuando yo multiplico un `Statevector` por un escalar (ej. un número como $\frac{\sqrt{3}}{2}$), el resultado es un nuevo `Statevector` cuyas amplitudes internas han sido escaladas por ese valor.


Por ende, la forma para poder pasarle un escalar custom a los `Statevector` de `"-"` y `"+"` es de esta forma:
```python
plus_state_amp = (math.sqrt(3)/2) * plus_state
minus_state_amp = (1/2) * minus_state
```

Ahora me pide crear el estado combinando ambas partes y verificar que está normalizado


> [!Tip] Algo para mi yo del futuro
> Las claves `'0'` y `'1'` del diccionario son arbitratias: representan los dos resultados posibles de medición en la base computacional.
> Cualquier estado cuántico de un solo qubit, sin importar cuán compleja sea su superposición (ya sea $|+\rangle$, $|-\rangle$, o el `new_state` que cree recién), al momento de ser medido en un laboratorio o simulador, va a colapsar obligatoriamente en uno de dos resultados clasicos: `0` o `1`. Por eso, `probabilities_dict()` siempre me muestra la probabilidad de observar `0` y la de observar `1` para ese estado global.
> Entonces cuando nosotros ecribimos un estado como $|+\rangle = \frac{1}{\sqrt{2}}|0\rangle + \frac{1}{\sqrt{2}}|1\rangle$, estamos viendo su **receta interna** en función de los estados base ($|0\rangle$ y $|1\rangle$). Entonces al hacer la medición, las amplitudes de esa receta determinan las chances de inclinar la balanza hacia el 0 o hacia el 1.

[[Otra cosa a tener en cuenta]]

---
# Bras y bases
## Vectores fila (bras)
En notación de Dirac, un **bra** $\left< u\right|$ representa un vector fila en $\mathbb{C}^n$:
$$
\begin{equation}
\left< u\right| =\begin{bmatrix}y_1 & \cdots & y_n\end{bmatrix}\in \mathbb{C}^n
\end{equation}
$$

Supongamos que tenemos un `ket` expresado como la combinación lineal de otros **kets:** $\left|\psi\right>=\alpha\left|\psi_1\right>+\beta\left|\psi_2\right>$. Entonces, podemos obtener su *bra asociado* $\left< \psi\right|$ tomando su **conjugado hermítico**, representado por el símbolo $\dagger$. Aplicar el conjugado hermítico a un `ket` (vector columna) lo vuelve un bra (vector fila), y aplicarlo a una constante devuelve el conjugado complejo de esa constante ($z^\dagger=z^*$):
$$
\begin{equation}
\left< \psi\right|=\left|\psi\right>^\dagger=(\alpha\left|\psi_1\right>+\beta\left|\psi_2\right>)^\dagger=\alpha^*\left< \psi_1\right|+\beta^*\left< \psi_2\right|
\end{equation}
$$
Cuando tomamos una combinación lineal dentro de un bra, hay que tener en cuenta que las constantes "salen conjugadas":
$$
\begin{equation}
\left< \alpha\psi_1+\beta\psi_2\right|=\alpha^*\left< \psi_1\right|+\beta^*\left< \psi_2\right|
\end{equation}
$$
**Ejemplo:** para un qubit en el estado $|\psi\rangle=\alpha|0\rangle+\beta|1\rangle=(\alpha,\beta)$, su bra asociado está dado por $\langle\psi|=\alpha^*\langle0|+\beta^*\langle1|=\begin{bmatrix} \alpha^* & \beta^*\end{bmatrix}$

## Productos internos (brakets)
No podemos sumar peras con manzanas: podemos sumar kets entre sí, y bras entre sí, pero no kets con bras. Sin embargo, sí podemos multiplicarlos.

¿Qué sucede cuando multiplicamos un bra $\left< w\right|$ por un ket $\left| v \right>$? Obtenemos un **braket** $\left< w|v\right>$: un número complejo que representa el *producto interno* entre los vectores columna $\left| w\right>=\begin{pmatrix} w_1, \ldots , w_n \end{pmatrix}$ y $\left| v\right>=\begin{pmatrix} v_1, \ldots , v_n \end{pmatrix}$. Los vectores deben tener las mismas dimensiones para realizar esta operación.
$$
\begin{equation}
\left< w|v\right>=\begin{bmatrix}w_1^* & \cdots & w_n^*\end{bmatrix}\begin{bmatrix}v_1 \\ \vdots \\ v_n\end{bmatrix}=w_1^*v_1+\cdots+w_n^*v_n\in\mathbb{C}
\end{equation}
$$
El producto interno de un vector no nulo con sí mismo siempre es positivo: $\left< v|v\right>>0$; sólo puede ser $0$ si el vector es nulo, $\left| v\right>=\begin{pmatrix} 0, \ldots , 0\end{pmatrix}\equiv\vec{0}$.
En computación cuántica, el producto interno de un qubit con sí mismo siempre es igual a uno: $\left< \psi|\psi\right>=1$. Esta condición es equivalente a pedir que las probabilidades sumen uno: $|\alpha|^2+|\beta|^2=1$.
$$
\begin{equation}
\left< \psi|\psi\right>=\begin{bmatrix}\alpha^* & \beta^*\end{bmatrix}\begin{bmatrix}\alpha \\ \beta\end{bmatrix}=\alpha^*\alpha+\beta^*\beta=|\alpha|^2+|\beta|^2=1
\end{equation}
$$
## Bases ortonormales
Una *base ortonormal* es un conjunto de vectores ortogonales y de norma unitaria que generan un espacio vectorial.

Por ejemplo, supongamos que la base $\mathbb{B}=\left\{\left|u_1\right>, \left|u_2\right>, \ldots , \left|u_n\right> \right\}$ es una base ortonormal de $\mathbb{C}^n$. Entonces, el producto interno entre dos kets distintos de la base valdrá $0$, y será $1$ para cuando sea entre kets idénticos:
$$
\begin{equation}
\left< u_i |u_j\right> =
\begin{cases}
1, & i = j \\
0, & i \neq j
\end{cases}
\end{equation}
$$

En computación cuántica, es muy común escribir la superposición de estados de un qubit como la combinación lineal de alguna base ortonormal:
$$
\begin{equation}
\left|\psi\right>=\sum_{i=1}^n c_i \left|u_i\right>
\end{equation}
$$

Para el caso de un qubit, vimos que podemos escribir la superposición como la combinación lineal de los estados de la base computacional: $\left|\psi\right>=\alpha\left|0\right>+\beta\left|1\right>$. También podemos expresar una superposición respecto a otras bases ortonormales, como $\left\{\left|+\right>,\left|-\right>\right\}$ ó $\left\{\left|+i\right>,\left|-i\right>\right\}$ (ver el ejercicio 2).

También podemos expresar dos qubits en simultáneo como una combinación lineal de una base ortonormal. Lo veremos en detalle más adelante, pero es útil tener un entendimiento básico ahora.

Dos bits pueden estar en los estados $00$, $01$, $10$ y $11$. Análogamente, un qubit puede estar en los estados $\left|00\right>$, $\left|01\right>$, $\left|10\right>$, $\left|11\right>$, o en una combinación lineal (o superposición) de ellos:
$$
\begin{equation}
\left|\psi\right>=\alpha_{00} \left|00\right>+\alpha_{01}\left|01\right>+\alpha_{10}\left|10\right>+\alpha_{11}\left|11\right>
\end{equation}
$$

Al tener cuatro estados de la base computacional, podemos representarlos vectorialmente de la siguiente manera:
$$
\begin{equation}
\left|00\right>=\begin{bmatrix} 1\\ 0 \\ 0 \\ 0\end{bmatrix}
\qquad\qquad
\left|01\right>=\begin{bmatrix} 0\\ 1 \\ 0 \\ 0\end{bmatrix}
\qquad\qquad
\left|10\right>=\begin{bmatrix} 0\\ 0 \\ 1 \\ 0\end{bmatrix}
\qquad\qquad
\left|11\right>=\begin{bmatrix} 0\\ 0\\ 0\\ 1\end{bmatrix}
\end{equation}
$$

Este mismo razonamiento puede extenderse para cualquier número de qubits. Para $N$ qubits, podremos superponer $2^N$ estados. Para tan solo $500$ qubits, tendremos $2^{500}$ amplitudes de probabilidad: ¡más que el número estimado de átomos en el universo! En parte, el gran poder de procesamiento de las computadoras cuánticas proviene de este hecho.

### Ejercicios
**a.** Obtén el bra asociado al qubit del ejercicio 1 y muéstralo como un arreglo.
```python
#Escriba su código aquí para la parte a
from qiskit.quantum_info import Statevector
amplitudes = [(1 + 1j) / 2, (1 - 1j) / 2]
qubit_1 = Statevector(amplitudes)
```

Ok no pude indagar mucho pero en resumen, el bra, ademas de cambiar la orientación de **columna** a **fila**, aplicamos **conjugado complejo** (invertir el signo de la parte imaginaria)

En Qiskit, el objeto `Statevector` almacena sus amplitudes en una propiedad llamada `.data`, la cual devuelve un arreglo de `NumPy`. Como el bra requiere las amplitudes conjugadas, podemos usar el método `.conjugate()` de `NumPy` sobre ese arreglo.
```python
#Escriba su código aquí para la parte a

from qiskit.quantum_info import Statevector

amplitudes = [(1 + 1j) / 2, (1 - 1j) / 2]
qubit_1 = Statevector(amplitudes)

qubit_conjugated = qubit_1.data.conjugate()
print(qubit_conjugated)
```
**Me dio:** `[0.5-0.5j  0.5+0.5j]`
Es un vector fila en donde cada amplitud original ha sido transformada por su conjugado complejo.

Ahora para el caso de la parte b, necesito usar el método `.inner()` para computar el **producto interno** entre el `qubit_1` original y el estado de superposición uniforme $|+\rangle$

En computación cuántica, el [[producto interno]] entre dos estados (ej `qubit_1` y el estado $|+\rangle$) es una operación matemática que mide qué tan **similares** o **superpuestos** están entre si.
- El resultado es un único número complejo que actúa como una especie de "puntuación de parecido"
- Si dos estados son idénticos, su producto interno da 1 (máxima similitud)
- Si son opuestos o perpendiculares (ortogonales), da 0 (ninguna similitud).

El método `.inner()` toma el **brá** del primer estado y lo multiplica por el **ket** del segundo estado.

Por un momento iba a realizar el pasaje de un **ket** a un bra y **aplicar** la conjugación dentro del mismo y después multiplicarlo por el ket del estado de superposición uniforme. Pero como el ejercicio me pide que utilice el método `inner()` para poder conseguir el [[producto interno]], lo que hice fue investigar en la documentación de `IBM` la forma en la cual puedo calcular el producto interno llamándolo desde la clase `Statevector` y pasándole como parámetro el `statevector` de '+'. 

```python
from qiskit.quantum_info import Statevector
amplitudes = [(1 + 1j) / 2, (1 - 1j) / 2]
qubit_1 = Statevector(amplitudes)

#Escriba su código aquí para la parte b
plus_state = Statevector.from_label('+')
resultado = qubit_1.inner(plus_state)
print(resultado)
```

**Resultado:**
`(0.7071067811865475+0j)`

**Ejercicio 4:**
Para sacar el producto interno entre dos vectores cumple que $\langle v|w \rangle = \langle w | v \rangle^*$ . Muestra esa propiedad computando $\langle i | - \rangle y \langle - | i \rangle$

Lo que hice fue simplemente hacer al revés, con eso veo que aplicando `inner` al revés (O sea, llamando la función de `inner()` usando el primer estado para el segundo y viceversa) dan lo mismo.


> [!Tip] Otro tip
> Cuando ejecutas `vector1.inner(vector2)`, Qiskit hace dos cosas de forma interna:
> 1. Convierte el primer vector en su **bra** (lo pasa a vector fila y le aplica el conjugado complejo a sus amplitudes).
> 2. Lo multiplica por el ket del segundo vector.
>    Por eso, al cambiar el orden (`right_sv.inner(minus_sv)` frente a `minus_sv.inner(right_sv)`), el conjugado se aplica automáticamente al nuevo vector que quedó a la izquierda, cumpliendo la regla matemática $\langle v \vert{} w \rangle = \langle w \vert{} v \rangle^*$.

**Ejercicio 5: norma de un ket**
La **norma** de un vector es un número real dado por $|||v\rangle||=\sqrt{\langle v|v\rangle}$. Verifica que la norma del qubit del ejercicio 1 tenga norma unitaria utilizando un método apropiado. Crea el vector columna $|w\rangle=(\sqrt{2},1)=\sqrt{2}|0\rangle+|1\rangle$ y verifica, utilizando el mismo método, que su norma no es unitaria.

En criollo, cuando calculamos $\langle v\vert{}v\rangle$, estamos multiplicando el **bra** del vector por su propio **ket**. Al aplicarle la raíz cuadrada $\sqrt{\dots}$ obtenemos la norma total, que debe dar $1$ exactamente para que el estado sea válido y represente probabilidades reales.

He visto que existe una función la cual valida que el `Statevector` sea norm 1. Lo cual significa que se encuentra normalizado: `is_valid`
```python
# Ahora debemos calcular su norma
norm = qubit_1.is_valid()
print(norm)
```

O si quiero ver exactamente el valor cuantificable de la norma, puedo utilizar la librería de `numpy` para calcular la raíz cuadrada:
```python
import numpy as np

# Calculamos la norma usando el producto interno y la raíz cuadrada de NumPy
norm = np.sqrt(qubit_1.inner(qubit_1))
print(norm)
```

**Y para verificar que la norma no es unitaria:**
En computación cuántica, que la **norma** de un estado no sea unitaria significa que la longitud total del vector (calculada a través de su producto interno y raíz cuadrada) no es igual a 1.

```python
w = Statevector([np.sqrt(2), 1])
print(f"es la norma |w> unitaria? {w.is_valid()}")
```



### Ejercicio 6
#### Ejercicio 6 - Inciso A
Nos pide crear 2 estados $\frac{\sqrt{2}|00\rangle+|11\rangle}{4}+\frac{\sqrt{26}}{8}(|01\rangle+|10\rangle)$, y tengo que verificar que esté [[normalizado]], y después obtener las probabilidades de obtener los estados `00`, `01`, `10` y `11`
$$\frac{\sqrt{2}}{4}\vert{}00\rangle + \frac{\sqrt{26}}{8}\vert{}01\rangle + \frac{\sqrt{26}}{8}\vert{}10\rangle + \frac{1}{4}\vert{}11\rangle$$
Esto sería en pocas palabras. Por ende creamos `stateVector`'s para cada uno junto con sus amplitudes.

Primero cree cada qubit por separado, con sus respectivas amplitudes:
```python
qb1 = (sqrt(2) / 4) * Statevector.from_label("00")

qb2 = (sqrt(26) / 8) * Statevector.from_label("01")

qb3 = (sqrt(26) / 8) * Statevector.from_label("10")

qb4 = (1/4) * Statevector.from_label("11")
```

Después lo que hice fue juntarlos todos utilizando sus respectivos signos (todos "+"):
```python
qb_general = qb1 + qb2 + qb3 + qb4
```

**Y por último agarré el diccionario de probabilidades y lo imprimí individualmente:**
```python
probs = qb_general.probabilities_dict()
for key, value in probs.items():
	print('{}: {}'.format(key, value))
```

Y con eso me aseguro de que estoy imprimiendo cada llave con su respectivo valor dentro del diccionario que la query me soltó.

#### Ejercicio 6 - Inciso B
_b. Crea el estado de superposición uniforme de $3$ qubits: $\frac{1}{\sqrt{8}}\sum_{i=0}^{7}\vert{}i\rangle$, donde $\vert{}i\rangle$ es un estado de la base computacional de $3$ qubits escrito en formato decimal (por ejemplo, $\vert{}7\rangle$ representa el estado $\vert{}111\rangle$)._

Primero que nada recordemos sobre la definición de [[Superposición Uniforme]].
**En el notebook nos menciona que el estado $\vert i \rangle$ está en decimal:** 

| **Índice decimal i** | **Estado en 3 qubits (Binario)** |
| -------------------- | -------------------------------- |
| $0$                  | $\vert{}000\rangle$              |
| $1$                  | $\vert{}001\rangle$              |
| $2$                  | $\vert{}010\rangle$              |
| $\vdots$             | $\vdots$                         |
| $7$                  | $\vert{}111\rangle$              |

Por ende cuando a la clase `Statevector([...])` le paso una lista de 8 amplitudes, el índice de la lista le asigna la amplitud al estado decimal correspondiente:
- **Posición 0** de la lista sería para $\vert 000 \rangle$
- **Posición 7** de la lista es para $\vert 111 \rangle$

**Algo para recordar por las dudas:**
- Base computacional: Son simplemente estados clásicos que sirven como bloques para construir. Para 1 qubit son $\{\vert{}0\rangle, \vert{}1\rangle\}$. Para 3 qubits son los 8 estados desde $\vert{}000\rangle$ hasta $\vert{}111\rangle$.
- **La ventaja de `Statevector`:** En lugar de hacer la suma manual término por término multiplicando cada label por ($\frac{1}{\sqrt{8}}$), le puedo parar una lista de 8 amplitudes y Qiskit asigna automáticamente cada posición del arreglo a su correspondiente estado de la base computacional.
- **Superposición uniforme:** Al ponerle a las 8 posiciones la amplitud $\frac{1}{\sqrt{8}}$, logro que todos tengan exactamente un $1/8$ de **probs**.

**Algo que encontré en internet fue que puedo hacer esto:**
Crear una lista de 8 amplitudes iguales multiplicando una lista de un elemento: `[1 / np.sqrt(8)] * 8`.

#### Ejercicio 6 - Inciso C
Crea una función para preparar una superposición equiprobable de $N$ estados para $n$ qubits: $\frac{1}{\sqrt{N}}\sum_{i=0}^{N-1}|i\rangle$, donde $N=2^n$.

Primero que nada hay que recordar qué es una [[Superposición Equiprobable]]






















