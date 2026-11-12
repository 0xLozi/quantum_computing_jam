# Mediciones y observables
## Operadores hermíticos
Los **operadores hermíticos** son operadores lineales cuyo conjugado hermítico es igual al mismo operador; es decir, se cumple que ${\hat{A}}^\dagger=\hat{A}$. Si tomamos su representación matricial $A$, vemos que se cumplen dos importantes propiedades (sin importar la base que elijamos para construir la matriz):

- *Sus autovalores son reales.* Recordemos que los autovalores $\lambda_i$ de una matriz se obtienen resolviendo la ecuación $\left|A-\lambda I\right|=0$, donde $I$ es la matriz identidad.

- *Sus autovectores son ortogonales entre sí.* Podemos obtener el autovector $|\phi_i\rangle$ asociado a cada autovalor $\lambda_i$ resolviendo la ecuación matricial $\left(A-\lambda_iI\right)|\phi_i\rangle=\vec{0}$.

## Observables
En mecánica cuántica, las magnitudes físicas que podemos medir (como la posición, la cantidad de movimiento, el spin o el momento angular) se denominan **observables**. Cada observable $\mathcal{A}$ se describe mediante un operador hermítico $\hat{A}$. Al medir el observable $\mathcal{A}$, obtendremos como resultado los autovalores del operador hermítico.

El espectro de autovalores del operador puede ser tanto continuo como discreto. Si es discreto, puede ser tanto finito como infinito. Si el espectro de autovalores es discreto, diremos que el observable está **cuantizado**.

**Ejemplo:** una propiedad de ciertas partículas es el spin. Cuando medimos el spin de un [fermión](https://es.wikipedia.org/wiki/Fermi%C3%B3n#:~:text=Un%20fermi%C3%B3n%20es%20uno%20de,principio%20de%20exclusi%C3%B3n%20de%20Pauli.) a lo largo de una dirección cualquiera, obtenemos uno de dos valores posibles. Supongamos que esa dirección se llama "eje $z$". El observable que representa la proyección del spin sobre el eje $z$ es $\mathcal{S}_z$, y su operador hermítico $\hat{S}_z$ tiene la siguiente representación matricial:
$$
\begin{equation}
S_z=\frac{\hbar}{2}\begin{bmatrix}1 & 0 \\ 0 & -1 \end{bmatrix}
\end{equation}
$$

donde $\hbar$ es una [constante física](https://es.wikipedia.org/wiki/Constante_de_Planck).
Los dos resultados posibles que obtendremos al medir son los autovalores de la matriz: $+\frac{\hbar}{2}$ (asociado al autoestado/autovector conocido como "spin up") y $-\frac{\hbar}{2}$ (asociado al autoestado "spin down").


## Proyectores
Un grupo importante de operadores hermíticos son los *operadores de proyección*, o **proyectores**. Si aplicamos un proyector $\hat{P}_{|\varphi\rangle}=|\varphi\rangle\langle\varphi|$ al ket $|\pi\rangle$, obtenemos la *proyección ortogonal* de $|\pi\rangle$ sobre el *subespacio* generado por $|\varphi\rangle$.

Los proyectores satisfacen la igualdad $\hat{P}^2=\hat{P}$. Esto quiere decir que si aplicamos un proyector dos veces, obtendremos el mismo resultado que al aplicarlo una sola vez.

Al aplicar $\hat{P}_{|\varphi\rangle}$ a $|\pi\rangle$, el ket se proyecta sobre el subespacio generado por $|\varphi\rangle$: $\hat{P}_{|\varphi\rangle}|\pi\rangle=|\varphi\rangle\langle\varphi|\pi\rangle=z|\varphi\rangle$, donde $\langle\varphi|\pi\rangle=z\in\mathbb{C}$. La proyección da como resultado el ket $|\varphi\rangle$ multiplicado por un número $z$. Nótese que el vector resultante no necesariamente está normalizado.

En general, podemos armar un operador de proyección sumando proyecciones ortogonales. El resultado será un operador que, al aplicarlo a un ket, lo proyecte al subespacio generado por el conjunto de las proyecciones individuales:
$$
\begin{equation}
\hat{P}_m=\sum_{i=1}^m|\phi_i\rangle\langle\phi_i|
\end{equation}
$$

## Mediciones
Cada par de autovalor y autovector de un operador hermítico cumple la famosa ecuación de autovalores y autovectores:

$$
\begin{equation}
\hat{A}|\phi_i\rangle=\lambda_i|\phi_i\rangle
\end{equation}
$$

En el caso de que cada autovalor esté asociado a un único autovector, podemos saber cuál es la probabilidad de medir el autovalor $\lambda_i$ en el sistema representado por el estado $|\psi\rangle$ mediante la siguiente ecuación:

$$
\begin{equation}
P(\lambda_i)=\left|\langle\phi_i|\psi\rangle\right|^2
\end{equation}
$$

En el caso de que un autovalor $\lambda_j$ sea compartido por muchos autovectores (de manera tal que cada uno cumpla por su cuenta con la ecuación de autovalores y autovectores), estamos ante un problema **degenerado**: no podemos distinguir un único autovector o **autoestado** al medir el autovalor. Si un autovalor tiene asociados $m$ autovectores, decimos que el problema es *m-degenerado*, siendo $m$ el grado de degeneración.

Para los problemas degenerados, la probabilidad de medir el autovalor $\lambda_j$ en un sistema en el estado $|\psi⟩$ está dada por:

$$
\begin{equation}
P(\lambda_j)=\langle\psi|\hat{P}_m|\psi\rangle
\end{equation}
$$

donde $\hat{P}_m$ es el proyector generado por todos los autovectores asociados al autovalor $\lambda_j$:

$$
\begin{equation}
\hat{P}_m=|\phi_{j1}\rangle\langle\phi_{j1}|+|\phi_{j2}\rangle\langle\phi_{j2}|+\cdots+|\phi_{jm}\rangle\langle\phi_{jm}|
\end{equation}
$$

¿Cuál será el estado del sistema después de medir? Luego de realizar una medición, el ket *colapsa* al estado $|\psi'\rangle$, que está dado por la siguiente ecuación:

$$
\begin{equation}
|\psi'\rangle= \frac{\hat{P}_i|\psi\rangle}{\sqrt{P(\lambda_i)}}
\end{equation}
$$

Para el caso no degenerado, $\hat{P}_i=|\phi_i\rangle\langle\phi_i|$.

Dicho en palabras (aunque para algunas personas pueda traer más confusión que esclarecimiento), el estado después de medir será la proyección ortogonal al subespacio asociado al autovalor que se midió, después de normalizarlo.





