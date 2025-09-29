# Q-GRAN: Quantum-Enhanced Graph Neural Network Framework

This is the official repository for the **Q-GRAN** research project. The project aims to develop hybrid Quantum-Classical algorithms, specifically Quantum-Enhanced Graph Neural Networks, to solve complex optimization problems in Space-Air-Ground Integrated Networks (SAGINs).

---

## 🔬 Research Environment

To ensure 100% reproducibility, the entire research environment is encapsulated within a Docker image. This image contains a complete, pre-configured toolchain with GPU support for all major quantum computing and machine learning libraries.

### Prerequisites
- A Linux-based host machine
- [Docker Engine](https://docs.docker.com/engine/install/)
- An NVIDIA GPU
- [NVIDIA Driver](https://www.nvidia.com/Download/index.aspx)
- [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

### Quick Start Guide

1.  **Pull the Docker image from Docker Hub:**
    ```bash
    docker pull haodpsut/qgran:latest
    ```

2.  **Run the container and start a session:**
    Navigate to your local project directory on the host machine and execute the following command:
    ```bash
    docker run --gpus all -it --rm -v "$(pwd)":/workspace haodpsut/qgran:latest /bin/bash
    ```
    - `--gpus all`: Grants the container access to all available NVIDIA GPUs.
    - `-it`: Runs the container in interactive mode with a pseudo-TTY.
    - `--rm`: Automatically removes the container when you exit the session.
    - `-v "$(pwd)":/workspace`: Mounts your current host directory into the `/workspace` directory inside the container, allowing you to edit code locally and run it inside the container.

3.  **Verify the environment:**
    Once inside the container's shell, run the verification script to ensure all components are functioning correctly:
    ```bash
    python check_env_final.py
    ```
    The output should confirm that both PyTorch and PennyLane are successfully utilizing the GPU.

---

## ❤️ The Journey: A Guide to Overcoming Installation Hell

Building this environment was a formidable challenge. This section documents the painful lessons learned, hoping to save others from the same struggle.

*   **The `Illegal instruction` Nightmare:** The root cause was pre-compiled Python wheels (`.whl`) on PyPI, especially for `PennyLane-Lightning`, which were built with modern CPU instruction sets (AVX/AVX2). These are incompatible with older server-grade CPUs. **Lesson:** When in doubt, compile from source (`--no-binary`) with safe flags (`-mno-avx`).

*   **The Conda vs. Pip Civil War:** Attempting to mix complex binary packages from different Conda channels (`conda-forge`, `pytorch`) with `pip` created an unstable environment plagued by low-level C++ library conflicts (`undefined symbol` errors) and CUDA visibility issues (`CUDA available: False`). **Lesson:** For complex GPU environments, avoid mixing package managers for core binary dependencies.

*   **The Qiskit 0.x vs. 1.x Schism:** Trying to maintain a Qiskit 0.x ecosystem led to unsolvable dependency conflicts, particularly with modern versions of `qiskit-ibm-runtime`. **Lesson:** The quantum ecosystem moves fast. Embrace the latest major stable release (Qiskit >= 1.0) to avoid legacy issues.

*   **The Ultimate Solution: Docker:** Docker proved to be the silver bullet. It provides a pristine, consistent Ubuntu base, isolating the environment from the host system's quirks. This allows for complete control over the installation process, from system-level tools to Python packages, guaranteeing reproducibility and eliminating "it works on my machine" problems.

---
