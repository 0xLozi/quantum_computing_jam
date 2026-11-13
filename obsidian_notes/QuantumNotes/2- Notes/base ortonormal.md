**Otra nota relacionada:**
[[ortonormal]]


Es un conjunto de vectores que cumple dos condiciones a la vez: son [[perpendiculares]] entre sí y **miden 1**. Con ellos podés construir cualquier estado del qubit:

|palabra|qué significa|cómo se comprueba|
|---|---|---|
|**Ortogonales**|“perpendiculares”: no comparten nada|$\langle u_i\vert u_j\rangle=0$ si $i\neq j$|
|**Normales**|cada vector mide 1|$\langle u_i\vert u_i\rangle=1$|
|**Base**|con ellos se puede armar cualquier estado|para un qubit hacen falta 2 vectores|

**Las dos condiciones juntas son la fórmula del notebook:**
$$\large{\langle u_i|u_j\rangle=\begin{cases}1&i=j\\0&i\neq j\end{cases}}$$
### Ejemplos
- $\{|0\rangle,|1\rangle\}$ (base computacional): $\langle0|1\rangle=1\cdot0+0\cdot1=0$.
- $\{|+\rangle,|-\rangle\}$ (base de Hadamard): $\langle+|-\rangle=\frac12(1\cdot1+1\cdot(-1))=0$.
- $\{|i\rangle,|-i\rangle\}$ (base circular): $\langle i|-i\rangle=\frac12(1\cdot1+(-i)(-i))=\frac12(1-1)=0$.

**No** es ortonormal $\{|0\rangle,|+\rangle\}$: ambos miden 1, pero $\langle0|+\rangle=\frac{1}{\sqrt2}\neq0$, así que no son ortogonales.

### Para qué sirve en computación cuántica
- **Descomponer un estado:** cualquier $|\psi\rangle=\sum_i c_i|u_i\rangle$, y los coeficientes se obtienen con $c_i=\langle u_i|\psi\rangle$.
- **Medir:** la probabilidad de cada resultado es $|c_i|^2$. Por eso, medir en la base $\{|+\rangle,|-\rangle\}$ es distinto de medir en la computacional.
- **Distinguir estados:** estados ortogonales se pueden distinguir con certeza, como vimos con $|+\rangle$ y $|-\rangle$.
