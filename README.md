# qrng-hackathon_2025
Contains my project presented at the Qiskit Fall Fest PNC 2025 for the Quantum Random Number Generator hackathon.

## Project structure
```
qrng-hackathon_2025/
├── README.md
└── qrng/
    ├── circuit.py
    ├── runner.py
    └── hackathon_2025_usage.ipynb
```
    
## Explanation
### circuit.py
Contains function build_qrng which generates a circuit creating a uniform superposition of all outcomes.

### runner.py
Contains function run_qrng. This function runs the circuit generated with build_qrng and returns a random number.

The user can decide whether to use a real quantum backend or Aer local simulator.
Real backend is run with error mitigation/suppression techniques, whereas Aer includes a NoiseModel simulating the noise affecting a real backend.

The function returns a random integer number within the chosen range, together with different information on the output random distribution,
including itsn normalized standard deviation to quantify its randomness (ideally, standard deviation = 0).

#### Working principle
run_qrng creates a uniform superposition of quantum states from |0> to |2^num_qubits-1> by applying an Hadamard gate to each qubit, then measures all the qubits with the chosen backend type. The measurement is repeated _shots_ times, where _shots_ is user-defined. The function then discards the undesired outputs, i.e., the outcomes such that _num_outcomes_ -1 < outcomes <= 2^num_qubits -1, where _num_outcomes_ is the user-defined number of outcomes desired (an integer random number is generated in the range [0, num_outcomes-1]). Among the remaining results, the most frequent outcome is chosen as random number.

If two or more otucomes are equally most frequent, the function re-run the circuit with a single shot, and output the obtained outcome as random number. This avoids chosing a criterion which would affect the randomicity of the algorithm; for example, a criterion such as "in case of equally most frequent outcomes pick the smallest one" would drive the generated random numbers towards low outputs. Furthermo, by re-running the circuit only in case of equally most-frequent outcome, the resource consumption of the algorithm is reduced

###  hackathon_2025_usage.ipynb
Contains a usage example of run_qrng. The function is first run by using a real backend, than using AerSimulator. Finally, the uniformity of the two so-generated distributions is compared by looking at their normalized standard deviations.
