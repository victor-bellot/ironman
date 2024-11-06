# Paper's notes

## Differentiable Force Closure

Grasp synthesis:
- analytic approach
- data driven approach

DoF = Degrees of Freedom

A **force-closure** grasp ensures resistance to any wrench.
It's a set of contact points (subject to friction or not) constraining a rigid body.

Determining if a grasp is a **force-closure** is a computationally expensive problem. Moreover, real contacts aren't isolated and therefor generate more resisting wrenches.

Using relaxation, they translate the **force-closure** problem into a minimization problem of a scalar function encoding 3 constraints:
- full rank
- friction cone
- surface belonging

To integrate hand pose, the problem is then translated into the estimation of the hand pose probability distribution associated with the grasping energy function. The hand pose (and its contact points) is therefor iteratively estimated using a Langevin dynamics model. Updates are done using a Metropolis method.

## Generalizable Dexterous Grasping

GenDexGrasp treats the **contact-map** as a transferable and intermediate representation for hand-agnostic grasping. That's a map indicating distances the hand should be from each points of the object.

Differentiable Force Closure (DFC) is used to create a dataset of **contact-map** from different hands, objects and poses.

Then a CVAE is trained on this dataset, conditioned by object's models.

Therefor at inference time, a standard normal distribution is sampled and only an object's model is required to predict a **contact-map**.

Finally, given a hand, the problem of grasping along the predicted **contact-map** is translated into a differentiable optimization problem and is solved using gradient descend.

## Geometry Matching for Multi-Embodiment Grasping

From the GenDexGrasp's dataset, a collection of (hand point cloud with **N** key-points, object point cloud) is associated with contact points on object surface. The model used to estimate this complex function is as followed :
- extract point clouds geometry using Graph Neural Networks
- match key-points to contact points in an auto-regressive manner (using MLP)

Critics :
- this paper is supposed to be about Multi-Embodiment Grasping, but it doesn't contain any analysis of inference with unseen grippers
- there is no reason why the predicted contact points should correspond to any physical reality (the system doesn't take into account joint constraints, nor ensure contact points satisfy force closure)

## Dexterous Grasping Network

DFC improvements:
- initialization strategy (T, R, joints)
- penetration energy the other way around
- replace *prior* energy by *self-penetration* and *joint* energies
- in the MALA algorithm, use gradient descend instead of using Langevin
- to compute SDF, use Kaolin instead of DeepSDF

## Grasp It!

A 2004 paper introducing a software for grasp simulation.
It models hand and object collision and friction.
It evaluates the quality of a grasp (a set of unit punctual forces) by compute its **Grasp Wench Space** (GWS) as a convex hull.

"
In general, a grasp exists to perform some task,
which can be described by the space of wrenches that
must be applied to an object to carry out that task.
This space is called the task wrench space.
If the task wrench space is a subspace of the grasp wrench
space, then the grasp is valid for that task. However, it is not
necessarily the most efficient grasp that can accomplish the
task, meaning that the internal forces on the object may be
much larger than necessary.
"

This paper is not about how to generate grasps.
They used a brute-force algorithm get feasible grasp for grasp point sampling.
A controller is proposed to execute a given grasp dynamically.

## Contact Grasp

Goal : functional grasp synthesis

"
This motivates the development of an object-centric approach
that is not tied to a specific end-effector, and that emphasizes
the reproduction of demonstrated contact.
"

Contact map = map random points at the object's surface to -1 (repulsive) or +1 (attractive).

From 3D model of objects, use GraspIt! to propose some starting grasps.
Then using contact maps from human demonstration, optimize the grasps energetically defined as :
- A grasp term = asset if the hand pose matches the contact map.
- A thumb contact term = manually defined a point at the hand surface as thumb, try to ensure contact of this point with the object's surface.
- An intersection term = to avoid hand / object intersection and hand self-intersection.
Optimization is done using the Levenberg-Marquadt algorithm from DART.
Rank the grasp, and select the best ones.

Limits :
- given a gripper, an object and a purpose, how to generate a functional contact map?

## Fast-Grasp’D

Improvement of [Grasp'D](https://link.springer.com/chapter/10.1007/978-3-031-20068-7_12).
Grasp synthesis using a differentiable simulation-based metric.

The metric assesses the stability of a grasp by simulating the movement of the object from different starting velocities. The goal is to minimize its final velocities. Moreover, joint constraints are taken into account in the global loss.

This paper describes how they improve their differentiable grasping simulator to make it runs fast : integration scheme, Signed Distance Function, leaky gradient...

## D-Grasp

Inputs:
- 6D estimated pose of the object
- static grasp reference
- 6D target pose of the object

Output : generate a grasping motion to move the object to the targeted pose

RL in 2 stages:
- The grasping phase : the hand needs to approach an object and find a physically-plausible and stable grasp.
- The motion synthesis phase : the hand has to bring the object into the 6D target pose while the grasping policy retains a stable grasp on the object.

Key idea : extract relevant features from (current hand/object poses and target object pose) to feed in the grasping policy network. 

## Grasp Taxonomy

Human grasp classification:
- power : can't move the object without moving the arm
- precision : can manipulate the object with the fingers
- intermediate : a bit of both

Opposition types (X, Y, Z from the hand rule : not to be confused with the fingers rule!):
- pad opposition (force apply along the finger's axis X)
- palm opposition (force apply along the palm's axis Y)
- side opposition (force apply along the thumb's axis Z)

Virtual finger : cluster fingers into functional groups (ie. pinch has 2 VF).

Thumb function:
- adducted: thumb applies a force along the Y axis
- abducted: other cases

They classified human grasp into 33 categories!

## Eigen Grasp

Largely inspired by *Postural hand synergies for tool use, 1998* : human reason in a low dimensional grasping space (PCA argument).
If a gripper has a lot a DoF, only a subset of all the possible poses produces efficient grasps.

Grasp planning:
- work in an 8 dimensional space (2 eigen-grasps + 6 DoFs of the wrist)
- define an energy function based on predefined on-gripper contact points, contact normal, and GWS metric
- use the Simulated Annealing algorithm to find a grasp minimizing energy

Critics:
- there isn't any grasp transfer between grippers
- the grasp planner requires the user to define contact points manually

## Contact-Invariant Optimization

Based on an under-constraints optimizer.
Given physics constraints, try to minimize some cost.
Cost takes into account Contact-Invariant, Physics/Kinematic Violation and Task.

To make to optimization problem easier (more continuous), a contact point distribution (N points) is determined every Dt=0.5s and is considered as constant during that period.

"
We use cubic spline interpolation for quantities x, p (using ẋ and ṗ as spline tangents), linear interpolation for f, r_object, and constant interpolation for c.
"

The wrist pose is represented as x, p the poses of fingertips (joint's DoFs are then determined using inverse kinematic).

Critics:
- how to properly choose N
- what impact Dt, and the interpolation have on the physical realism
- long-term planning + slow optimization -> nice for animation, not robotics

## Grasp Synthesis — A Survey

2 kinds of algorithm : analytical (geometry, physics) and empirical (iterative, bio-inspired) approaches.

Analytical:
- Force-closure grasp synthesis : find a set of contact points satisfying force-closure.
- Optimal force-closure grasps : define metric to determine which grasp is the best.
- Task compatibility : take into account which wrench we want to apply using the grasped object.
- Issues : computationally expensive, solve the problem statically, case specific (no generalization to other gripper, object, task...).
- Advantages : ensure force-closure, enable task criteria.

Nice take away : Task Wrench Space (TWS) -> see *Grasp planning: how to choose a suitable task wrench space, Borst et al. 2004*.

Empirical:
- Learning-by-Demonstration : easier for task specification.
- Object-based : determine grasping patterns at the object's surface.

Take away : *A new strategy combining empirical and analytical approaches for grasping unknown 3D objects, S. El-Khoury, A. Sahbani 2010*.

Both algorithm's types suffer from the same problem : task modeling.

## Data-Driven Grasp Synthesis — A Survey

Analytic approaches guarantee force-closure,
however these are usually based on strong assumptions distant from practical uses.
- Exact computations perform in simulation based on accurate models.
- Lack of robustness : can't handle noisy conditions.

There is a need for interaction with the environment and dynamic consideration.

*Empirical or data-driven approaches rely on sampling grasp candidates for an object and ranking them according to a specific metric.*

GraspIt! (2004) paved the way toward empirical approaches by providing a grasping framework. Sample & Rank methods emerge. Most of them use the $\epsilon$-metric based on the Grasp Wrench Space (GWS). However, recent studies show that simulation validated grasps are fragile : the $\epsilon$-metric performs poorly in practice.

We cannot ensure the stability of static grasps : it's a dynamic property.
Therefor force-closure is not enough.

For this reason, experience learning based methods have been developed.
The expectation is that robustness and generalization will be learned.

Real world implementations require 3D vision system, object detection, pose estimation, feature extraction...

## Mathieu GROSSARD

A set of papers in collaboration with:
- Interactive Robotics Unite of CEA-LIST
- Signals and Systems Laboratory of Centrale Supélec
- ESME Research Lab, ESME Sudria Engineering school

They have a practical and mechanical approach.

### New Metric for Wrench Space Reachability of Multi-fingered Hand with Contact Uncertainties

This paper introduces Reachable Wrench Space under Uncertainties (RWSU) to account for geometric uncertainties (that only affect the grasp map G). It aims to handle error on the contact surface normal, the object location and the hand kinematic model.

The RWSU is defined as the intersection of all the possible Wrench Space given the uncertainties. This polytope is then approximated by computing its lower and upper bounds.

Critics:
- examples are only performed on a really simple case
- these are no uncertainties entanglement

### Energy-based stability analysis for grasp selection with compliant multi-fingered hands

An energy-based analysis of the stability of grasp is proposed taken into account the kinematic model of the multi-fingered hand used (in particular its joint gains). It is done by analyzing the stiffness of the object at the contact's points to determine slipping criteria.

Remarks:
- simple 2D example...
- this kind of method maybe really effective combined with an appropriate control

## Fundamentals of Grasping

### Grasping as an optimization problem under the force-closure constraint

Given a set of $k$ contact points at the object's surface and an estimation of the friction coefficient $\mu$, we can compute the friction cones $F_i$ associated at each contact point. Let's $C$ be the actuator constraint. The problem is to find a set of force vectors $\{f_i\}_{i=1}^k$ satisfying force-closure and such that $f_i \in F_i$ and $f_i \in C$ for $i=1, \dots , k$. This problem can then be solved by formulating it as a convex optimization problem :
$$
\text{minimize}_{f_i , i \in \{1,\dots,k \}} \quad J (f_1, \dots , f_k) \\
\text{s.t.} \quad f_i \in F_i \cap C, i \in \{1, \dots , k\} \\
\text{and} \quad w − G \left[ f_1 \dots f_k \right]^\mathbf{T} = 0
$$
where $J$ is the objective function that is convex in each $f_i$. A common choice is to take the maximum of the force's norms.

### Learning-Based Approaches to Grasping

- Choosing a Grasp Point from an RGB Image
- Exploiting Simulation and a Database of Objects to Predict Grasp Success
- Learning to Grasp Through Real-World Trial-and-Error

### Learning-Based Approaches to Manipulation

- Planar Pushing
- Contact-Rich Manipulation Tasks

## UniDexGrasp: Universal Robotic Dexterous Grasping via Learning Diverse Proposal Generation and Goal-Conditioned Policy

Dexterous grasping from a point cloud observation under a table-top setting (a realistic robotics setting).

It's a really complicated setup! Each piece is based on entire papers...

### Grasp Orientation Generation

- GraspIPDF estimates $P(R|X_0)$
- from a sampled $R$, GraspGlow estimates $P(\tilde{t},q|\tilde{X}_0)$ with $\tilde{X}_0=R^{-1}X_0$
- then the canonical grasp $(R,\tilde{t},\tilde{X}_0)$ is optimized using ContactNet
- we finally get $(R,t,q)$ with $t=R\tilde{t}$

### Goal-Conditioned Dexterous Grasping Policy

- learn a state-based teacher policy
- distill to the vision-based student policy

## Sparse Distilled Feature Field

For each point of view, a featured point cloud is computed using DINOv2.
The obtained features are then refinement and aggregated using Hough voting.
Therefor, we get a representation of the object.

Given a demonstration (object + grasp), points are sampled at the gripper surface.
The optimization process tries to minimize the difference between the features at those points on the demonstration and the real hand.

## CNOS

To match scene view to dataset meshes, they compute the cosine similarity between class tokens given by DINOv2.

## Point Cloud understanding

### PointNet

Pre-process point cloud to take permutation invariance into account. Generate local and global features.

### PointNet++

Apply PointNet feature extraction at different scale to ensure multi-scale understanding. Similar to CNN : aggregate points by proximity (inside a metric ball).

## Contact-GraspNet

6 DOF parallel-jaw grasp generation from depth point cloud.

Using PointNet++, point-wise features are computed. They encode both the local and the global situations of the points. Among a small subset of points, heads are train to predict (contact confidence, gripper orientation, grasp width).

A grasp can be selected considering its confidence value and its physical correctness in the given situation.

## Self-Distillation Vision Transformers with No Labels (DINO)

From a huge RGB image dataset, train a student to predict the same features as a teacher based on different parts of a given image.
The teacher's parameters are updated as a moving average of the optimized student's parameters.
Both models are vision transformers : a feature is predicted for each image's patches. A class token is also learned in parallel of the feature.

## DexGraspNet 2.0

PointNet++ like auto-encoder -> local feature -> diffusion model -> variational (T, R) -> determininistic $\theta$

Nice graspness definition of a point.
