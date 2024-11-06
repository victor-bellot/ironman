# Resources

## Lessons

[Grasping History](https://www.techniques-ingenieur.fr/base-documentaire/automatique-robotique-th16/conception-modelisation-et-commande-en-robotique-42398210/prehension-robotique-et-manipulation-dextre-s7765/)

[Force closure in robotics](https://modernrobotics.northwestern.edu/nu-gm-book-resource/12-2-3-force-closure/)

[Sammy Christen at the Advanced Interactive Technologies lab (ETH Zurich)](https://ait.ethz.ch/people/sammyc)

[Binding C++ with Python](https://pybind11.readthedocs.io/)

## Repositories

[Original DFC repository](https://github.com/tengyu-liu/diverse-and-stable-grasp)

[DexGraspNet DFC's version](https://github.com/PKU-EPIC/DexGraspNet)

[Multi-grasp repository](https://github.com/MultiGrasp/MultiGrasp)

[RobotiQ 3-finger gripper URDF](https://github.com/ros-industrial/robotiq/tree/kinetic-devel/robotiq_3f_gripper_visualization/cfg)

[Grasp'D](https://github.com/dylanturpin/graspd)

[D-Grasp](https://github.com/christsa/dgrasp)

[Blender Proc](https://github.com/DLR-RM/BlenderProc/blob/main/README_BlenderProc4BOP.md)

[Contact GraspNet](https://github.com/NVlabs/contact_graspnet)

## Interesting

[Contact-Invariant Optimization for Hand Manipulation](https://www.youtube.com/watch?v=Gzt2UoxYfAQ)

François Chollet : Abstraction and Reasoning Corpus for Artificial General Intelligence (ARC-AGI).

[VGN: Real-Time 6 DOF Grasp Detection in Clutter](https://www.youtube.com/watch?v=FXjvFDcV6E0)

- IDQL: Implicit Q-Learning as an Actor-Critic Method with Diffusion Policies
- OpenAI Soft Actor-Critic

## Grippers

- [Shadow Hand](https://www.shadowrobot.com/dexterous-hand-series)
- [RobotiQ-3f](https://robotiq.com/products/3-finger-adaptive-robot-gripper)
- Franka Panda robot with a parallel-jaw gripper
- [OpenBionics bio-inspired hand](https://openbionics.org/robothands)

## Notion

### 3d transformation

[Rodrigues rotation formula](https://en.wikipedia.org/wiki/Rodrigues%27_rotation_formula)

### Bayes

[Notation](https://en.wikipedia.org/wiki/Bayesian_inference)

### CVAE

Auto-Encoder (AE) : learn a synthetic representation of high dimension data points (encoder - latent space - decoder)

Variational AE (VAE) : learn a distribution explaining the data (variation can be generated using sampling)

Conditional VAE (CVAE) : learn data conditionally (enable control on the sampling later)

The original [paper](https://proceedings.neurips.cc/paper_files/paper/2015/hash/8d55a249e6baa5c06772297520da2051-Abstract.html)

Mathematical approach [here](https://beckham.nz/2023/04/27/conditional-vaes.html)

### Optimization technics

- Markov Chain Monte Carlo methods based on Langevin dynamics [2nd derivative]
- Levenberg–Marquardt algorithm (Gauss-Newton x Trust region) [1st derivative]
- Simulated Annealing : a Metropolis–Hastings like algorithm -> to optimize a high dimensional function [no derivative]
- Broyden–Fletcher–Goldfarb–Shanno algorithm [WTF]
- Hebbian theory -> Hopfield network -> Boltzmann machine

### Computer graphics

[Views sampled on a sphere](https://github.com/thodan/bop_toolkit/blob/master/bop_toolkit_lib/view_sampler.py)

### Classical problem

- Multi-armed bandit
- Optimal transport
- Inverse problem
- Shapley's game theory
- Persistent Homology
- Braess's paradox
