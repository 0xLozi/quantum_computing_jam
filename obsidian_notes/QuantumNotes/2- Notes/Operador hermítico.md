Un operador es **hermítico** si es igual a su propio adjunto:

$$\hat A^\dagger=\hat A$$

Es la misma operación de siempre, transponer y conjugar, pero ahora la matriz queda **igual**.

## Cómo se reconoce en una matriz

- **La diagonal es real.** Cada elemento $a_{ii}$ debe ser igual a su conjugado, y eso solo pasa con números reales.
- **Los elementos opuestos respecto de la diagonal son conjugados:** $a_{ij}=\overline{a_{ji}}$.

## Ejemplos
$Y=\begin{bmatrix}0&-i\\i&0\end{bmatrix}$ es hermítica. Al transponer queda $\begin{bmatrix}0&i\\-i&0\end{bmatrix}$ y al conjugar vuelve a $\begin{bmatrix}0&-i\\i&0\end{bmatrix}$, que es $Y$. Con $X$, $Z$ y $H$ pasa lo mismo.


$\begin{bmatrix}0&i\\i&0\end{bmatrix}$ **no lo es**: los elementos de afuera de la diagonal son iguales entre sí, pero deberían ser conjugados ($i$ y $-i$). Al transponer y conjugar queda $\begin{bmatrix}0&-i\\-i&0\end{bmatrix}$, que es distinta.
## Para qué sirven
Los hermíticos representan **observables**, o sea cosas que se miden:

- **Sus autovalores son reales.** Los resultados de una medición son números reales. Demostración corta: si $A|v\rangle=\lambda|v\rangle$, entonces $\langle v|A|v\rangle=\lambda\langle v|v\rangle$. Pero $\langle v|A|v\rangle$ es igual a su propio conjugado (porque $A^\dagger=A$), así que es real, y $\lambda$ también.

- **Autovectores de autovalores distintos son ortogonales**, y por eso funciona la descomposición espectral $A=\sum_i\lambda_i|u_i\rangle\langle u_i|$ del ejercicio 13.

- **El valor medio $\langle\psi|A|\psi\rangle$ es real** (ejercicio 14).

## Relación con lo unitario

|           | hermítico      | unitario                  |
| --------- | -------------- | ------------------------- |
| condición | $A^\dagger=A$  | $U^\dagger U=I$           |
| rol       | lo que se mide | cómo evoluciona el estado |
Si $\hat H$ es hermítico, entonces $e^{-i\hat Ht}$ es unitario. $X,Y,Z,H$ son las dos cosas a la vez, y $S$ y $T$ son unitarios pero no hermíticos.