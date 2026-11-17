**Ok entonces tenemos esto:**
Después de aplicar $R_Y(\theta)$ a $|0\rangle$, el estado es:
$$
\large{\cos\tfrac{\theta}{2}\,|0\rangle+\sin\tfrac{\theta}{2}\,|1\rangle}
$$
Cada número que multiplica a un ket es la **amplitud** de ese ket:
- la amplitud de $|0\rangle$ es $\cos\frac{\theta}{2}$,
- la amplitud de $|1\rangle$ es $\sin\frac{\theta}{2}$.

O sea son simplemente dos números que dependen del ángulo $\theta$
El enunciado pide $P(0)\approx14.6\%$, o sea $0.146$. (Y $14.6\%+85.4\%=100\%$.) La ecuación entonces es:
$$
\large{\cos^2\tfrac{\theta}{2}=0.146}
$$
Así es como despejo $\theta$: 
$$
\large{\sqrt{\cos^2\tfrac{\theta}{2}}=\sqrt{0.146}} \newline
\large{\cos\tfrac{\theta}{2}=\sqrt{0.146}} \newline
\text{Ahora aplico coseno inverso} \newline
\tfrac{\theta}{2}=\arccos\!\big(\sqrt{0.146}\big) \newline
\sqrt{0.146} \approx 0.3821 \newline
\frac{\theta}{2} = \arccos(0.3821) \newline
\frac{\theta}{2} \approx 1.1787\text{ rad} \newline
\theta \approx 2 \cdot 1.1787 \approx 2.3575\text{ rad} \newline
\frac{\theta}{\pi} \approx 0.7504 \approx \frac{3}{4} \implies \theta = \frac{3\pi}{4}
$$
#### Importante
- **La esfera de Bloch es así:**
    - El **Polo Norte** es el estado $\vert{}0\rangle$ (ángulo $0$).
    - El **Polo Sur** es el estado $\vert{}1\rangle$ (ángulo $\pi$, o $180^\circ$).
    - El **Ecuador** es la mitad exacta: $50\%$ de probabilidad de $\vert{}0\rangle$ y $50\%$ de $\vert{}1\rangle$ (ángulo $\frac{\pi}{2}$, o $90^\circ$).
- **Lo único que hace `RY(θ)` es inclinar la flecha desde el Polo Norte hacia el Sur:**
    - Si inclinas poquito ($\theta$ chico), la flecha queda cerca del Norte $\rightarrow$ casi siempre sale $\vert{}0\rangle$.
    - Si la inclinas más allá del ecuador ($\theta = \frac{3\pi}{4}$, que son $135^\circ$, tres cuartos de camino hacia el Sur), la flecha queda bien abajo $\rightarrow$ por eso sale $85.4\%$ de veces $\vert{}1\rangle$ y solo $14.6\%$ de veces $\vert{}0\rangle$.

[[Guardar para poder sacar ángulos más fácil]]
[[Diferencias entre Qubit y Qubit Clásico]]


# Ejercicio 10
Para un algoritmo VQE (Variational Quantum Eigensolver) se uitliza el modo Session

Motivo principal:
- Dependencia entre pasos: En VQE, un optimizador clásico calcula un nuevo conjunto de parámetros basándose en el resultado de la ejecución cuántica anterior, y envía ese nuevo circuito a ejecutarse una y otra vez hasta converger.
- Por qué session es el indicado: Abre una ventana dedicada en el procesador cuántico donde todos los trabajos consecutivos del bucle se ejecutan sin tener que volver a hacer fila en la cola global del sistema en cada iteración

- Por qué no los otros:
    - Job: Ejecutaría cada iteración como una tarea aislada, obligando a esperar en la cola pública en cada paso del bucle
    - Batch: Sirve para enviar muchos circuitos independientes todos juntos de una sola vez (en paralelo), pero en VQE no conocés los ángulos de la iteración N + 1 hasta que no termina la iteración N

# Ejercicio 11
- **Sampler:** Lo que hace es ejecutar el circuito muchas veces (shots) y lo que hace es devolver la distribución de probabilidades (o conteos) de cada resultado binario medido (un ejemplo cuando antes al graficar habia salido `00` vs `01`, y veías la cantidad de veces que salió).

- **Estimator:** Toma el circuito junto con un observable físico (un operador matemático, como un Hamiltoniano de energía) y calcula directamente su valor esperado promedio ($\langle \psi \vert{} \hat{H} \vert{} \psi \rangle$)), sin la necesidad de reconstruir las probabilidades manualmente.





# Ejercicio 14
En una computadora cuántica física, los qubits **son extremadamente frágiles**. Si un qubit queda esperando sin hacer nada (por ejemplo mientras otro qubit está ejecutando una operación lenta) **el entorno puede interferir y hacer que pierda su estado cuántico antes de tiempo.** Esa pérdida de información por el paso del tiempo se llama **decoherencia**. 

Dentro del enunciado se presentan tres herramientas de **Qiskit** para combatir distintos tipos de errores:
- **Readout Error Mitigation:** la cual limpia los fallos que ocurren únicamente en el instante final al medir el qubit; es decir, cuando leemos un 1 en vez de un 0, por ejemplo. 

- **Dynamical Decoupling:** introduce pulsos de control rápidos durante los tiempos muertos de espera para mantener al qubit activo y protegerlo contra la pérdida de información del entorno. 

- **Zero Noise Extrapolation:** el cual ejecuta el circuito varias veces amplificando el ruido a propósito para luego estimar matemáticamente cuál habría sido el resultado sin ruido. 


**Respuesta al ejercicio:**
La técnica más adecuada es el **dynamical decoupling** o **desacomplamiento dinámico**. Dicha técnica de supresión de errores inserta secuencias de pulsos de control como pares de compuertas X sobre los qubits durante sus tiempos de inactividad (es decir "idling") manteniendo su estado activo y cancelando la interferencia del entorno para evitar la decoherencia antes de que ocurra la medición. 


# Ejercicio 16
Una característica fundamental de lógica clásica incorporada en OpenQASM 3 —y ausente en OpenQASM 2— es el **flujo de control clásico** (_classical control flow_), que incluye estructuras **condicionales (`if / else`)** y **bucles o iteraciones (`for`, `while`)**. Esto permite evaluar resultados de mediciones clásicas en tiempo real (_feedforward_) para decidir qué compuertas cuánticas ejecutar o repetir dinámicamente durante la ejecución del circuito.


# Ejercicio 18
- **API REST (Interfaz de Programación de Aplicaciones REST):** Es un puente de comunicación basado en los protocolos estándar de la web (**HTTP/HTTPS**) que permite que dos sistemas escritos en distintos lenguajes intercambien datos (generalmente en formato JSON) mediante métodos como `POST` (para enviar tareas) y `GET` (para consultar resultados).
- **Relación con el Ejercicio 18:** Como el SDK oficial de Qiskit está hecho para Python, un servidor en Node.js o Go no puede importarlo directamente; en su lugar, usa la `API REST` de Qiskit Runtime como intermediario para enviar circuitos cuánticos a la nube de IBM.
- **Autenticación:** Para que IBM acepte esas peticiones HTTP y sepa a qué cuenta facturar o asignar los trabajos, cada solicitud debe incluir una credencial secreta única: tu API Token (o API Key).




