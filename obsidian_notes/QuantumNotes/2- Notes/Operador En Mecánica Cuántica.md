Un operador en mecánica cuántica (y algebra lineal) es una función o instrucción matemática representada por una matriz.

Su trabajo principal es **tomar un estado cuántico (un Ket $\vert{}\psi\rangle$** y transformarlo en un nuevo estado (otro Ket).

### Visualmente hablando
**Si el estado cuántico es un vector columna:**
$$\large{\vert{}\psi\rangle = \begin{bmatrix} a \\ b \end{bmatrix}}$$
**El operador es una matriz cuadrada:**
$$\hat{A} = \begin{bmatrix} a_{00} & a_{01} \\ a_{10} & a_{11} \end{bmatrix}$$
Cuando el operador actúa sobre el estado, simplemente hacemos una multiplicación de matriz por vector:
$$\hat{A} \vert{}\psi\rangle = \begin{bmatrix} a_{00} & a_{01} \\ a_{10} & a_{11} \end{bmatrix} \begin{bmatrix} a \\ b \end{bmatrix} = \text{Nuevo Ket } \vert{}\psi'\rangle$$

**Cosas a tener en cuenta sobre qué hace un operador:**
1. **Transformar o cambiar el estado:** Como las compuertas cuánticas (ej. voltear un $\vert 0 \rangle$ para convertirlo en $\vert 1 \rangle$)
2. **Representar una medición:** Extraer información del sistema cuántico.
3. **Proyectar estados:** Filtrar o seleccionar una parte del estado.

