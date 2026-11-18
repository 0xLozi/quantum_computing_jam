- 🎛️ **¿Qué es el qubit de control?** En una compuerta controlada (como un `if` cuántico), es el qubit que "manda" si se aplica o no una operación sobre otro qubit (el objetivo). Como lo ponemos en superposición ($\vert{}+\rangle$), actúa como una condición viva donde ambas ramas ($\vert{}0\rangle$ y $\vert{}1\rangle$) conviven a la vez.

- **¿Qué es el _Phase Kickback_ y por qué transferimos el ángulo hacia el control?** Cuando el qubit objetivo es un **autovector** $\vert{}v\rangle$ de la compuerta $U$, aplicarle $U$ no cambia su estado, solo lo multiplica por su autovalor $e^{i\theta}$. Al ejecutar la compuerta controlada, esa fase $e^{i\theta}$ aparece solo cuando el control es $\vert{}1\rangle$ y, por factor común, queda pegada al **qubit de control**. **¿Por qué hacemos esto?** Porque si la fase quedara multiplicando a todo el sistema por fuera (fase global), sería físicamente invisible e imposible de medir. Al transferirla a la rama $\vert{}1\rangle$ del control, la convertimos en una **fase relativa** entre $\vert{}0\rangle$ y $\vert{}1\rangle$, dejando el ángulo listo para ser leído.

- 🔄 **De ángulo oculto a factores primos (`IQFT` + Shor):**
    1. **IQFT:** Toma esos ángulos relativos acumulados en los qubits de control y los traduce de vuelta a bits medibles (`0` y `1`).
    
    2. **Algoritmo de Shor:** Con esos bits obtenemos una fracción que, mediante fracciones continuas, nos da el **período $r$**. Finalmente, una cuenta clásica con el máximo común divisor ($\text{mcd}(a^{r/2} \pm 1, N)$) nos entrega los factores primos de $N$.
---
- ⚽ **Phase Kickback:** Transfiere el autovalor al control y crea esa diferencia de ángulo (fase relativa) entre $\vert{}0\rangle$ y $\vert{}1\rangle$.
   
- 🔄 **IQFT:** Convierte esa diferencia de ángulo en bits medibles en la base computacional.

- 🔓 **Shor:** Toma esa medición para deducir el período $r$ y hallar los factores primos de $N$.
