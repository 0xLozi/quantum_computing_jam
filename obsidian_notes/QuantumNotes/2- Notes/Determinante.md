Es un número único que extraemos de una matriz cuadrada
Geométricamente podemos pensarlo como **el factor de escala de un área** (en una matriz de $2 \times 2$). Cuando una matriz transforma el espacio, el determinante te dice **cuánto se estira o se encoge el área de una figura geométrica.**

En el contexto de los autovalores y la ecuación $\vert{}A - \lambda I\vert{} = 0$, buscar que el determinante sea igual a cero significa que estamos buscando los valores de $\lambda$ para los cuales la matriz **colapsa el espacio** en alguna dirección (es decir, el área se vuelve cero y pierde una dimensión).

Para una matriz de $2 \times 2$ como $\begin{bmatrix} a & b \\ c & d \end{bmatrix}$, el determinante se calcula multiplicando en cruz: $ad - bc$.


Para nuestra matriz modificada con el autovalor $\lambda$, la ecuación del determinante es:
$$(a - \lambda)(d - \lambda) - bc = 0$$
Esto no significa que los términos individuales deban dar cero ni que tengan que igualar un número fijo por separado. Si sumamos $bc$ en ambos lados, la igualdad queda así:
$$(a - \lambda)(d - \lambda) = bc$$
Esto significa que el **producto de los elementos de la diagonal modificada** debe ser exactamente igual al **producto de los elementos fuera de la diagonal** ($b \cdot c$).