### 1. La fórmula para sacar el ángulo $\theta$ en Python
Cuando te den una probabilidad objetivo y no quieras despejar nada a mano, usá estas dos líneas según qué probabilidad te den:

- **Si te dan la probabilidad de $\vert{}0\rangle$** (ej: $14.6\% \rightarrow 0.146$):
```python
theta = 2 * np.arccos(np.sqrt(0.146))
```
- **Si te dan la probabilidad de $\vert{}1\rangle$** (ej: $85.4\% \rightarrow 0.854$):
```python
    theta = 2 * np.arcsin(np.sqrt(0.854))
```
_(Nota: Ambas dan exactamente el mismo $\theta$. Si querés ver si es una fracción linda de $\pi$, tirás `print(theta / np.pi)`)._

### 2. Qué compuerta usar para "mover" la flecha
Pensá la esfera de Bloch como un globo terráqueo donde arrancás siempre en el **Polo Norte ($\vert{}0\rangle$)**:
  

| **Compuerta** | **Código Qiskit** | **Qué hace en la esfera**                                                      | **Cuándo usarla**                                                                                  |
| ------------- | ----------------- | ------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------- |
| **`RY(θ)`**   | `qc.ry(theta, 0)` | Baja la flecha del Norte ($\vert{}0\rangle$) hacia el Sur ($\vert{}1\rangle$). | **Casi siempre** que te pidan cambiar las probabilidades de medir $0$ y $1$.                       |
| **`RX(θ)`**   | `qc.rx(theta, 0)` | Baja la flecha igual que `RY`, pero girando por el costado (eje X).            | Cambia probabilidades igual que `RY`, pero deja el estado en otra posición del ecuador.            |
| **`RZ(θ)`**   | `qc.rz(theta, 0)` | Gira la flecha como un trompo **sin subirla ni bajarla**.                      | **No cambia las probabilidades** de medir $0$ o $1$, solo cambia la "fase" (el ángulo horizontal). |
| **`H`**       | `qc.h(0)`         | Te lleva directo al Ecuador ($50\%$ y $50\%$).                                 | Atajo rápido para `RY(π/2)` (superposición exacta).                                                |
| **`X`**       | `qc.x(0)`         | Da vuelta la flecha de Norte a Sur ($100\%$ $\vert{}1\rangle$).                | Atajo rápido para `RY(π)` (el `NOT` cuántico).                                                     |

### 3. Tabla rápida de ángulos que aparecen siempre

Para que los reconozcas a simple vista sin hacer cuentas:
- $\theta = 0 \rightarrow$ **Polo Norte:** $100\%$ $\vert{}0\rangle$, $0\%$ $\vert{}1\rangle$
- $\theta = \frac{\pi}{3}$ ($60^\circ$) $\rightarrow$ $75\%$ $\vert{}0\rangle$, $25\%$ $\vert{}1\rangle$
- $\theta = \frac{\pi}{2}$ ($90^\circ$) $\rightarrow$ **Ecuador:** $50\%$ $\vert{}0\rangle$, $50\%$ $\vert{}1\rangle$
- $\theta = \frac{2\pi}{3}$ ($120^\circ$) $\rightarrow$ $25\%$ $\vert{}0\rangle$, $75\%$ $\vert{}1\rangle$
- $\theta = \frac{3\pi}{4}$ ($135^\circ$) $\rightarrow$ $14.6\%$ $\vert{}0\rangle$, $85.4\%$ $\vert{}1\rangle$ _(el de tu ejercicio)_
- $\theta = \pi$ ($180^\circ$) $\rightarrow$ **Polo Sur:** $0\%$ $\vert{}0\rangle$, $100\%$ $\vert{}1\rangle$
### 4. Plantilla base en Qiskit (Copiar, pegar y adaptar)
Este bloque te resuelve el 90% de los ejercicios de un solo qubit. Solo cambiás la parte de las compuertas:
```python
import numpy as np
from qiskit import QuantumCircuit
from qiskit.quantum_info import Statevector
from qiskit.visualization import plot_bloch_multivector

# 1. Crear circuito de 1 qubit (arranca siempre en |0⟩)
qc = QuantumCircuit(1)

# 2. Calcular ángulo (si te dan probabilidad) y aplicar compuerta
prob_0 = 0.146
theta = 2 * np.arccos(np.sqrt(prob_0))
qc.ry(theta, 0)

# (Opcional) Si después te piden moverlo más, agregás otra línea abajo:
# qc.rz(np.pi / 2, 0)

# 3. Sacar el estado y ver probabilidades exactas
estado = Statevector.from_instruction(qc)
probs = estado.probabilities_dict()

print("Ángulo en fracción de pi:", round(theta / np.pi, 3))
print(f"Probabilidad |0⟩: {probs.get('0', 0)*100:.1f}%")
print(f"Probabilidad |1⟩: {probs.get('1', 0)*100:.1f}%")

# 4. Dibujar esfera de Bloch (siempre al final de la celda en Jupyter/Colab)
plot_bloch_multivector(estado)
```

### 💡 Tip clave si te piden "aplicar una compuerta y DESPUÉS moverlo"
En Qiskit las compuertas se ejecutan **en el orden en que las escribís de arriba hacia abajo**:
1. Primero ponés la que te lleva a la inclinación deseada (`qc.ry(...)`).
2. En la línea siguiente ponés el segundo movimiento (por ejemplo `qc.rz(...)` para rotarlo alrededor del ecuador, u otro `qc.ry(...)`).
3. Recién **después** de poner todas las compuertas llamás a `Statevector.from_instruction(qc)` para sacar la foto final de cómo quedó la flecha.


# Alternativa
He creado una función para poder sacar el ángulo utilizando probabilidades

```python
def angulo_para_probabilidades(porcentaje, estado='0'):
    prob = porcentaje / 100
    if estado == '0':
        return 2 * np.arccos(np.sqrt(prob))
    else:
        return 2 * np.arcsin(np.sqrt(prob))

theta = angulo_para_probabilidades(14.6)               # Asume que es probabilidad de |0⟩
theta = angulo_para_probabilidades(85.4, estado='1')   # Si el enunciado te da la de |1⟩
```

En donde con esto se puede sacar probabilidades y entonces aplicar la puerta `ry` sin problemas.






