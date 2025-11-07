# Advanced Quantum Gates and Circuits with Qiskit

This repository provides an enhanced and extended implementation of quantum gates and circuits using the Qiskit library. It builds upon the fundamentals by introducing more advanced gates, sophisticated circuit constructions, and diverse simulation techniques. Each code block is accompanied by a detailed explanation to facilitate understanding.

## Table of Contents
- [Advanced Quantum Gates and Circuits with Qiskit](#advanced-quantum-gates-and-circuits-with-qiskit)
  - [Table of Contents](#table-of-contents)
    - [A Quick Refresher: The Basics](#a-quick-refresher-the-basics)
    - [Expanding the Toolkit: Additional Quantum Gates](#expanding-the-toolkit-additional-quantum-gates)
      - [1. Parameterized Gates: U, RX, RY, RZ Gates](#1-parameterized-gates-u-rx-ry-rz-gates)
      - [2. Multi-Qubit Gates: SWAP and Toffoli (CCX) Gates](#2-multi-qubit-gates-swap-and-toffoli-ccx-gates)
    - [Advanced Circuit Construction](#advanced-circuit-construction)
      - [1. Creating Custom Gates](#1-creating-custom-gates)
      - [2. Parameterized Quantum Circuits](#2-parameterized-quantum-circuits)
    - [Exploring Different Simulators](#exploring-different-simulators)

---

### A Quick Refresher: The Basics

Quantum circuits are the fundamental building blocks of quantum programs. They consist of quantum bits (qubits) and the quantum gates that operate on them.

Here is a simple two-qubit circuit that creates an entangled Bell state.

```python
# Import necessary libraries from Qiskit
from qiskit import QuantumCircuit, Aer, execute
from qiskit.visualization import plot_histogram, plot_bloch_multivector

# Create a quantum circuit with 2 qubits and 2 classical bits
qc_bell = QuantumCircuit(2, 2)

# Add a Hadamard gate on qubit 0 to create a superposition
qc_bell.h(0)

# Add a CNOT gate with qubit 0 as the control and qubit 1 as the target
qc_bell.cx(0, 1)

# Map the quantum measurement to the classical bits
qc_bell.measure(,)

# Draw the circuit
print("Bell State Circuit:")
print(qc_bell)
```

**Explanation:**
*   `QuantumCircuit(2, 2)` initializes a circuit with two qubits and two classical bits.
*   `h(0)` applies a Hadamard gate to the first qubit, putting it into a superposition.
*   `cx(0, 1)` applies a CNOT gate, creating entanglement.
*   `measure([0,1], [0,1])` measures the qubits and stores the results in classical bits.

---

### Expanding the Toolkit: Additional Quantum Gates

Beyond the basic gates, Qiskit offers a rich set of more advanced gates.

#### 1. Parameterized Gates: U, RX, RY, RZ Gates

These gates perform rotations and their action depends on classical parameters.

```python
import numpy as np

qc_param = QuantumCircuit(1)
qc_param.rx(np.pi/2, 0) # Rotate around X-axis by pi/2
qc_param.ry(np.pi/3, 0) # Rotate around Y-axis by pi/3
qc_param.rz(np.pi/4, 0) # Rotate around Z-axis by pi/4

print("Parameterized Gates Circuit:")
print(qc_param)

# Visualize the final state
backend = Aer.get_backend('statevector_simulator')
statevector = execute(qc_param, backend).result().get_statevector()
plot_bloch_multivector(statevector)
```

#### 2. Multi-Qubit Gates: SWAP and Toffoli (CCX) Gates

*   **SWAP Gate:** Swaps the states of two qubits.
*   **Toffoli Gate (CCX):** A doubly-controlled NOT gate, universal for classical computation.

```python
qc_multi = QuantumCircuit(3)

# Initialize the qubits to |011>
qc_multi.x(1)
qc_multi.x(2)
qc_multi.barrier()

# Apply a SWAP gate
qc_multi.swap(0, 1)
qc_multi.barrier()

# Apply a Toffoli gate
qc_multi.ccx(0, 1, 2)

print("Multi-Qubit Gates Circuit:")
print(qc_multi)
```

---

### Advanced Circuit Construction

#### 1. Creating Custom Gates

Group a sequence of gates into a single, reusable custom gate.

```python
# Create a small circuit for the custom gate
sub_circuit = QuantumCircuit(2, name='my_gate')
sub_circuit.h(0)
sub_circuit.cx(0, 1)
my_gate = sub_circuit.to_gate()

# Use the custom gate in a larger circuit
qc_custom = QuantumCircuit(3)
qc_custom.append(my_gate,)
qc_custom.append(my_gate,)

print("Circuit with a Custom Gate:")
print(qc_custom)
```

#### 2. Parameterized Quantum Circuits

Create circuits with placeholder parameters that can be assigned values later. This is essential for variational algorithms.

```python
from qiskit.circuit import Parameter

theta = Parameter('θ')
qc_pqc = QuantumCircuit(1)
qc_pqc.h(0)
qc_pqc.rz(theta, 0)

print("Parameterized Quantum Circuit:")
print(qc_pqc)

# Bind the parameter to a specific value
bound_circuit = qc_pqc.bind_parameters({theta: np.pi/2})
print("\nCircuit with Bound Parameter:")
print(bound_circuit)
```

---

### Exploring Different Simulators

Qiskit's `Aer` module provides several powerful backends for simulation.

*   **`qasm_simulator`:** Mimics a real quantum computer by performing repeated measurements (shots).
*   **`unitary_simulator`:** Calculates the unitary matrix that represents the circuit's transformation.

```python
qc_bell_no_meas = QuantumCircuit(2)
qc_bell_no_meas.h(0)
qc_bell_no_meas.cx(0, 1)

# 1. Use the qasm_simulator
qasm_sim = Aer.get_backend('qasm_simulator')
qc_to_run = qc_bell_no_meas.copy()
qc_to_run.measure_all()
result_qasm = execute(qc_to_run, qasm_sim, shots=1024).result()
counts = result_qasm.get_counts()
print("Counts from qasm_simulator:")
plot_histogram(counts)

# 2. Use the unitary_simulator
unitary_sim = Aer.get_backend('unitary_simulator')
result_unitary = execute(qc_bell_no_meas, unitary_sim).result()
unitary = result_unitary.get_unitary()
print("\nUnitary matrix from unitary_simulator:")
print(np.round(unitary, 3))
```