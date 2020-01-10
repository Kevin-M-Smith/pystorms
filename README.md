# pystorms: simulation sandbox for the evaluation and design of stormwater control algorithms
[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/python/black)

## Overview 

This library has been developed in an effort to systematize quantitative analysis of stormwater control algorithms.
It is a natural extension of the Open-Storm's mission to open up and ease access into the technical world of smart stormwater systems.
 Our initial efforts allowed us to develop open source and free tools for anyone to be able to deploy flood sensors, measure green infrastructure, or even control storm or sewer systems.
 Now we have developed a tool to be able to test the performance of algorithms used to coordinate these different sensing and control technologies that have been deployed throughout urban water systems.    

For the motivation behind this effort, we refer the reader to our manuscript [*pystorms*](https://dl.acm.org/citation.cfm?id=3313336). In general, this repo provides three components:

1. A library of `scenarios` that are built to allow for systematic quantitative evaluation of stormwater control algorithms, 
2. A stormwater hydraulic simulator named `pyswmm_lite` and forked heavily from OWA's [SWMM](https://github.com/kLabUM/Stormwater-Management-Model.git) and [pyswmm](https://github.com/kLabUM/pyswmm_lite.git), and
3. An `environment` script that links the `pyswmm_lite` simulator to the `scenarios`, and can be edited/updated by users who might want to interface the `scenarios` with other stormwater simulator software (the `environment` script is included in `pyswmm-lite`).

This is a alpha version of the library, eventually `pyswmm_lite` dependency would be deprecated in the favour of `pyswmm`

## Getting Started 

### Installation 

**Requirements**

- python 3+
- numpy
- pyswmm_lite

```bash 
pip install pyswmm_lite
pip install pystorms
```
Please raise an issue on the repository or reach out if you run into any issues installing the package. 

### Example 

Here is an example implementation on how you would use this library for evaluating the ability of a rule based control in maintaining the flows in a network below a desired threshold. 

```python 
import pystorms 
import numpy as np

# Define your awesome controller 
def controller(state):
	actions = np.ones(len(state))
	for i in range(0, len(state)):
		if state[i] > 0.5:
			actions[i] = 1.0
	return actions 
	

env = pystorms.scenarios.theta() # Initialize scenario 

done = False
while not done:
	state = env.state()
	actions = controller(state)
	done = env.step(actions)

performance = env.performance()

```

Detailed documentation can be found on the [webpage](https://klabum.github.io/pystorms/)