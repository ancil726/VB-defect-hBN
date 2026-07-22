# Hybrid Classical-Quantum Simulation of the $V_B^{-}$ Defect in hBN

Welcome to the repository for the computational simulation of the negatively charged boron vacancy ($V_B^{-}$) in hexagonal boron nitride (hBN). 

This project demonstrates a proof-of-concept pipeline that integrates classical *ab initio* Density Functional Theory (DFT) and exact multi-reference chemistry with near-term quantum algorithms to compute the Zero-Phonon Line (ZPL) of a solid-state quantum emitter.

##  Project Overview

The $V_B^{-}$ defect in hBN is a highly promising solid-state color center for applications in quantum sensing and quantum information processing. Exact classical simulation of strongly correlated defect systems is often computationally unfeasible. This project tackles that bottleneck using a **hybrid classical-quantum embedding approach**:
1. The bulk crystal environment is treated classically.
2. The highly correlated active space (the defect state) is mapped to a quantum computational pipeline.

##  Computational Pipeline

The workflow relies on a modular, four-step methodology bridging mean-field calculations and quantum state optimization:

*   **Classical Pre-processing (Quantum Espresso & PySCF):** 
    *   Periodic DFT (PBE functional) is used to relax a 49-atom supercell, capturing the $C_{3v}$ ground state and the Jahn-Teller distorted excited state.
    *   A finite 21-atom cluster is extracted and passivated.
    *   Restricted Open-Shell Hartree-Fock (ROHF) and State-Averaged CASSCF(10,6) calculations isolate the active space and extract the one- and two-electron interaction integrals.
*   **Hamiltonian Mapping (Qiskit):** Classical integrals are transformed into fermionic operators. A Parity Mapper with two-qubit reduction compresses the active space into an efficient 10-qubit Hamiltonian.
*   **Variational Quantum Optimization:** 
    *   A UCCSD ansatz strictly conserves particle number and spin.
    *   **VQE** (Variational Quantum Eigensolver) minimizes the ground state.
    *   **VQD** (Variational Quantum Deflation) resolves the excited optical state by penalizing the ground state overlap.
*   **ZPL Evaluation:** The quantum eigenvalues and classical core shifts are recombined to compute the absolute Zero-Phonon Line.

##  Key Results

Our hybrid variational quantum simulation (VQE/VQD) yielded a pure electronic ZPL of **1.497 eV**. 

This precisely matches our exact classical diagonalization (FCI) benchmark for the defined active space, proving the validity of the constrained UCCSD quantum ansatz. It also highlights the limitations of standard single-reference $\Delta$SCF methods, which rely heavily on error cancellation when dealing with severe self-interaction errors in wide-gap insulators.

##  Repository Structure

*   [`main.md`](main.md): The comprehensive, fully formatted project report including detailed theoretical background, orbital visualizations, band structure analysis, and complete code execution.
*   [`main.ipynb`](main.ipynb): The raw, executable Jupyter Notebook containing the full Python pipeline.
*   `hBN_defect_cluster_ground.xyz` / `excited.xyz`: Optimized Cartesian coordinates for the localized defect clusters.
*   `hBN_defect_integrals_CASSCF_ONLY.npz`: Extracted classical Hamiltonian integrals.

##  Dependencies

To execute the quantum simulation pipeline locally, the following core libraries are required:
*   `numpy` / `scipy`
*   `pyscf`
*   `qiskit`
*   `qiskit-nature`
*   `qiskit-algorithms`
