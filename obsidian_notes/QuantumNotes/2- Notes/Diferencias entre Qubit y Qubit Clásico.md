El qubit hace el trabajo cuántico y el bit clásico guarda el resultado de la medición (`qc.measure`) para poder leerlo y tomar decisiones con `qc.if_test()`.

| **Característica**          | **Qubit (Cuántico) ⚛️**                                                                                                 | **Bit clásico (Normal) 💻**                                    |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| **¿Qué valores guarda?**    | Puede estar en **superposición** ($0$ y $1$ a la vez) y **entrelazarse** con otros qubits.                              | Solo guarda un **`0` o un `1`** fijo (como en cualquier PC).   |
| **¿Cómo se manipula?**      | Con compuertas cuánticas (`qc.h()`, `qc.x()`, `qc.cx()`).                                                               | Con lógica tradicional (como leerlo dentro de `qc.if_test()`). |
| **¿Se puede leer directo?** | No, mientras está en estado cuántico no podemos ver su valor sin alterarlo.                                             | Sí, está diseñado justamente para ser leído y almacenado.      |
| **¿Cómo se conectan?**      | Mediante **`qc.measure(qubit, bit_clasico)`**, que colapsa el qubit y guarda la "foto" del resultado en el bit clásico. | Recibe el dato de `measure` en la lista `qc.clbits[...]`.      |
