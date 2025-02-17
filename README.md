# brain_simulations

## Overview

This repository is focused on virtual brain connectivity and neural stimulation modeling. It leverages The Virtual Brain (TVB) framework ([TVB](https://www.thevirtualbrain.org/tvb/zwei/home)), an advanced computational neuroscience tool designed to simulate large-scale brain dynamics using biologically plausible models. TVB provides a powerful platform for simulating brain activity based on real brain imaging data (fMRI, DTI, EEG, MEG), visualizing neural interactions, and generating visualizations of structural and functional connectivity.

## Features

- Simulates neural connectivity and stimulation effects.
- Implements computational models for brain network analysis.
- Provides visualization of brain activity and connectivity patterns.
- Can be used for neuromodulation research and brain-computer interface applications.

## Usage

### Load the Required Libraries
The first few cells of the notebook load essential libraries. Run the following code snippet:
```bash
import numpy as np
import scipy
import matplotlib.pyplot as plt
import networkx as nx
from tvb.simulator.lab import *
```
These libraries handle numerical computations, data visualization, and brain connectivity modeling.

### Initialize the Virtual Brain Model
The following code snippet initializes a default brain model using TVB:
```python
model = models.Generic2dOscillator()
coupling = coupling.Linear(a=0.015)
connectivity = connectivity.Connectivity.from_file()
```
models.Generic2dOscillator(): Defines a generic neural mass model for simulation.

coupling.Linear(a=0.015): Sets up the coupling strength between brain regions.

connectivity.Connectivity.from_file(): Loads the brain connectivity matrix.

### Set Up and Run the Simulation

The simulator is then configured and run:
```python
sim = simulator.Simulator(
    model=model,
    connectivity=connectivity,
    coupling=coupling,
    integrator=integrators.HeunStochastic(dt=0.1),
    monitors=[monitors.Raw()]
).configure()

(time, data), = sim.run()
```
simulator.Simulator(): Sets up the simulation environment.

integrators.HeunStochastic(dt=0.1): Defines a stochastic integration scheme.

monitors.Raw(): Captures raw neural activity data.

sim.run(): Executes the simulation.

### Visualize the Results

Use matplotlib to plot the results:
```python
plt.plot(time[:1000], data[:1000, 0, 0, 0])
plt.xlabel("Time (ms)")
plt.ylabel("Neural Activity")
plt.title("Simulated Brain Activity Over Time")
plt.show()
 ```
This generates a time series plot of neural activity from one brain region.

### Modify Stimulation Parameters

You can modify the external stimulation parameters by adjusting the external input:
```python
stimulus = patterns.StimulusArray()
stimulus.configure_waveform(frequency=10, amplitude=0.05)
 ```
This allows users to simulate different stimulation conditions and analyze their effects on brain connectivity.

### Interpret Connectivity Changes

To analyze how stimulation impacts connectivity, use:
```python
conn_matrix = connectivity.weights
plt.imshow(conn_matrix, cmap='hot', interpolation='nearest')
plt.colorbar()
plt.title("Brain Connectivity Matrix")
plt.show()
```
This visualization helps interpret connectivity changes across brain regions.

## Applications

Neuromodulation: Understanding how stimulation affects brain connectivity.

Brain-Computer Interfaces (BCI): Developing non-invasive neural control interfaces.

Neuroscience Research: Investigating brain network dynamics and function.
