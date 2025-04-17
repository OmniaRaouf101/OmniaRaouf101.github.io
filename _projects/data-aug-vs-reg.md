---
title: Quantum Computer Simulator
order: 40
image: quantum-simulator.png
---
This project was a deep dive into quantum computation, where I developed a fully-functional, GPU-accelerated **quantum statevector simulator** entirely in Python. The main goal was to simulate quantum algorithms efficiently and support modern quantum programming standards, bridging the gap between theoretical design and practical implementation.

One of the key features of this simulator is its support for [OpenQASM 3.0](https://openqasm.com/), a standard intermediate representation for quantum circuits. This includes features such as **mid-circuit measurements**, **classical control flow**, and **dynamic circuit execution**—essential components for simulating realistic quantum programs and hybrid quantum-classical algorithms.

Unlike traditional simulators which often struggle with flexibility or performance, this implementation leverages **GPU acceleration** to handle large quantum systems, while maintaining a clean and modular design. This makes it a valuable tool for prototyping new quantum algorithms and for educational purposes.

The simulator has been tested on a variety of benchmark circuits and demonstrates scalable performance as system size increases. By integrating advanced features like control flow and measurement feedback, it moves one step closer to simulating near-term quantum devices and supporting research in quantum error correction, variational algorithms, and beyond.
