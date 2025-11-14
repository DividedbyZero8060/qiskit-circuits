# Quantum Fourier Transform using Qiskit

This project demonstrates the implementation of the Quantum Fourier Transform (QFT) using Qiskit, an open-source quantum computing software development framework.

## What is the Fourier Transform?

The Fourier Transform is a powerful mathematical tool that decomposes a function or a signal into its constituent frequencies. [8, 5] It essentially transforms a signal from the time domain to the frequency domain, which often simplifies analysis. [6, 14] This technique is fundamental in many fields, including signal processing, image analysis, and engineering. [5, 8] For example, it can be used to analyze the notes in a musical recording or to filter out unwanted noise. [9, 6]

## The Quantum Fourier Transform (QFT)

The Quantum Fourier Transform is the quantum analog of the classical Discrete Fourier Transform. [1, 2] Instead of acting on a vector of numbers, the QFT acts on the amplitudes of a quantum state. [1] This transformation is a key component in many significant quantum algorithms, such as Shor's algorithm for factoring and quantum phase estimation. [1, 4] The primary advantage of the QFT is its efficiency; on a quantum computer, it can be performed exponentially faster than its classical counterpart. [1]

The QFT transforms quantum states between the computational (Z) basis and the Fourier basis. For a single qubit, this is equivalent to the Hadamard gate. [4, 7]

## Implementation in Qiskit

This notebook walks through the process of building a QFT circuit and its inverse from the ground up using Qiskit.

### Building the QFT Circuit

The implementation is broken down into a few key functions:

1.  **`qft_rotations(circuit, n)`**: This function recursively applies the core logic of the QFT. It consists of a Hadamard gate followed by a series of controlled phase rotations on the qubits. [4] Each rotation's angle is progressively halved.

2.  **`swap_registers(circuit, n)`**: The QFT algorithm naturally outputs the qubits in a reversed order. This function applies a series of SWAP gates to reverse the order of the qubits and obtain the correct final state.

3.  **`qft(circuit, n)`**: This function combines the rotation and swap operations to perform the full QFT on the first `n` qubits of a given circuit.

4.  **`inverse_qft(circuit, n)`**: This creates the inverse of the QFT circuit. The inverse QFT is crucial for algorithms where the QFT is used as an intermediate step, allowing a return to the computational basis. [7]

The notebook demonstrates these steps by:
*   Initializing a quantum state, for example, the state `|101⟩` which represents the number 5.
*   Applying the `qft` function to this state.
*   Visualizing the resulting statevector on Bloch spheres using `plot_bloch_multivector`.
*   Applying the `inverse_qft` function to show that the initial state `|101⟩` is recovered.
*   Preparing the circuit to be run on a real quantum backend or a simulator.

### Running on an IBM Quantum Backend

The code includes the necessary steps to run the circuit on a real quantum device through IBM Quantum. This involves:
*   Authenticating the account using `QiskitRuntimeService`.
*   Selecting the least busy backend.
*   Transpiling the circuit for the target backend to optimize the gates for the specific hardware.
*   Using a `BackendSampler` to run the circuit and get the measurement results.

![QiskIT IBM Runtime](https://unidebhu-my.sharepoint.com/:i:/g/personal/ashraf_mahdi8060_mailbox_unideb_hu/IQCq4pcEyb_iR7x7HZWjupywAdC9vanAY6qviZWiyPwVpFs?e=41Y91v)

### Results

When the initial state `|101⟩` is prepared, and the QFT followed by the inverse QFT is applied, the final measurement should ideally yield the state `101` with 100% probability. However, due to noise in real quantum hardware and statistical noise in simulators, the output is probabilistic, with the correct state having the highest probability.

![Probabilistic Output](https://unidebhu-my.sharepoint.com/:i:/g/personal/ashraf_mahdi8060_mailbox_unideb_hu/IQCoDn1NPcVfRqZnN-bmClpgAfjiouufFlEoyc99GcDRTR4?e=UWwN3b)

---

## Presentation Participants

| Name | Neptune ID |
| :--- | :--- |
| Ashraf Mahdi | F8FOX0 |
| Jawad Bin Jahangir | JF2A45 |
| Burak Işık | C9MMAE |