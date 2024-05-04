---
title: "Modeling Space Plasma"
permalink: /space_plasma/
author_profile: true
redirect_from:
  - /plasma
---

__Background and Motivations__

Questions concerning celestial bodies and their potential for life, operational constraints for visiting spacecraft, and geologic scale time evolution rely on understanding the interaction between these objects and their environments. Spacecraft have sampled the magnetic field and plasma properties near many planets, moons, and comets in our solar system, but these data provide only momentary snapshots of the dynamic, three-dimensional regions they inhabit. Moreover, in order to fully characterize the fundamental physical processes that govern an object's interaction with its surroundings, observations must be combined with a modeling framework to produce a cohesive, global picture of the interaction region.
 
 __Mathematical Introduction__
 
We can characterize the behavior of space plasma in our solar system through the time evolution of a particle distribution function in a six-dimensional phase space \\(f\_s(\mathbf{x},\mathbf{v},t)\\), where \\(f\_s\\) is the average particle number density of species \\(s\\) with velocity \\(\mathbf{v}\\) at position \\(\mathbf{x}\\) and time \\(t\\). Starting from \\(f\_s(\mathbf{x},\mathbf{v},t)\\) and assuming that no particles are created or destroyed (\\(\frac{\mathrm{d}f\_s}{\mathrm{d}t}=0\\)), a straightforward application of the Lorentz force and Newton's second law yields the collision-free form of Boltzmann's equation, also known as Vlasov's equation (for further reference, see "Space Plasma Physics" by Baumjohann and Treumann, 2022):

\begin{equation}
\label{eq:vlasov}
\frac{\partial f\_s}{\partial t} + \mathbf{v} \boldsymbol{\cdot} \frac{\partial f\_s}{\partial \mathbf{x}} + \frac{q\_s}{m\_s} \left( \mathbf{E} + \mathbf{v} \times \mathbf{B} \right) \boldsymbol{\cdot} \frac{\partial f\_s}{\partial \mathbf{v}} = 0 \hspace{0.4cm}.
\end{equation}

In this expression, \\(q\_s\\) is the particle charge, \\(m\_s\\) is the particle mass, \\(\mathbf{E}\\) is the electric field, and \\(\mathbf{B}\\) is the magnetic field. 

# Reducing Vlasov's equation to multi-fluid approach
While Vlasov's equation \ref{eq:vlasov} describes the dynamics of a non-collisional plasma population, this fully kinetic treatment is computationally very expensive at most scales of interest (including for moon-magnetosphere interactions). Indeed, this approach has never been used by itself to create a three-dimensional model of the interaction between a moon and its plasma environment. Rather than solving the characteristics of Vlasov's equation directly, we can instead adopt a fluid picture in order to reduce the degrees of freedom inherent to the kinetic approach. Moreover, if the characteristic scale of space and time of the plasma environment are large compared to the gyroradii and inverse gyrofrequency of a specific particle species, respectively, we can approximate Vlasov's equation without truncating any meaningful dynamics. One method is the multi-fluid approach, where each species \\(s\\) is prescribed a fluid treatment using a limited set of macroscopic quantities that are independent of individual particle velocity \\(\mathbf{v}\\).

# The velocity moments of distribution function
To generate these variables, we can integrate the product of the distribution function of each plasma species \\(f\_s\\) with the velocity \\(\mathbf{v}^{k}\\) raised to integer powers of \\(k\\) over all of the three-dimensional velocity space:

\begin{equation}
\label{eq:f\_moments}
M\_{k,s} = \int\_{}^{} \mathrm{d}^{3}v \; f\_s(\mathbf{x}, \mathbf{v}, t) \; \mathbf{v}^{k}\hspace{0.4cm} .
\end{equation}

In this expression, \\(M\_{k,s}\\) is the \\(k^{\mathrm{th}}\\) statistical velocity moment of the distribution function for species \\(s\\). The "zeroth" moment (\\(k=0\\)) is thus the simple integral of the distribution function over all of velocity space, which intuitively yields the number density \\(n\_{s}(\mathbf{x},t)\\) of species \\(s\\) at location \\(\mathbf{x}\\) and time \\(t\\). Evaluation of the first order moment \\(M\_{1,s}\\) is similarly straightforward and gives the product of the number density and the average velocity, or "bulk" plasma flow \\(\mathbf{u}\_{s}(\mathbf{x},t)\\):

\begin{equation}
\label{eq:f\_moments\_zero}
M\_{0,s} = n\_{s}(\mathbf{x},t) = \int\_{}^{} \mathrm{d}^{3}v \; f\_s(\mathbf{x}, \mathbf{v}, t)
\end{equation}

\begin{equation}
\label{eq:f\_moments\_one}
M\_{1,s} = n\_{s}(\mathbf{x},t) \mathbf{u}\_s(\mathbf{x},t) = \int\_{}^{} \mathrm{d}^{3}v \; f\_s(\mathbf{x}, \mathbf{v}, t) \; \mathbf{v}
\end{equation}

From these two macroscopic quantities, we can also define another useful physical parameter, the current density \\(\mathbf{j}(\mathbf{x},t)\\):

\begin{equation}
\label{eq:current}
\mathbf{j}(\mathbf{x},t) = \sum\_{s} \; \mathbf{j}\_s = \sum\_{s} \; q\_{s} \; n\_{s}(\mathbf{x},t) \; \mathbf{u}\_{s}(\mathbf{x},t) \hspace{0.4cm}.
\end{equation}

The expressions for higher order moments of the distribution function (\\(k \geq 2\\)) are not singularly defined. For example, when solving the second order moment (\\(k=2\\)), the term \\(\mathbf{v}^{2}\\) can represented as either \\((\mathbf{v} \boldsymbol{\cdot} \mathbf{v})\\) or \\((\mathbf{v} \boldsymbol{\otimes} \mathbf{v})\\), where (\\(\boldsymbol{\cdot}\\)) and (\\(\boldsymbol{\otimes}\\)) correspond to the inner (dot) and outer (Kronecker or tensor) products, respectively. Applying the inner product and substituting \\((\mathbf{v}-\mathbf{u}\_{s})\\) for the velocity provides the "kinetic" temperature, a measure of the spread of the distribution function in velocity space about the average velocity \\(\mathbf{u}\_s\\). An analogous treatment for the outer product case yields the pressure tensor (the mass \\(m\_s\\) of species \\(s\\) is required in this expression to obtain the correct units of pressure):

\begin{equation}
\label{eq:f\_moments\_two}
M\_{2,s} = \boldsymbol{P}\_{s}(\mathbf{x},t) = m\_s \int\_{}^{} \mathrm{d}^{3}v \; f\_s(\mathbf{x}, \mathbf{v}, t) \; (\mathbf{v}-\mathbf{u}\_s) \boldsymbol{\otimes} (\mathbf{v}-\mathbf{u}\_s) \hspace{0.4cm}.
\end{equation}

Revisiting Vlasov's equation \ref{eq:vlasov} equipped with the relationship between these macroscopic quantities and the distribution function, we can now construct a single fluid (magnetohydrodynamic) or multi-fluid picture for characterizing plasma dynamics. As with the distribution function, the product of Vlasov's equation and velocity \\(\mathbf{v}^k\\) raised to integer powers of \\(k\\) can be integrated over velocity space to obtain equations of state that are independent of the velocity:

\begin{equation}
\label{eq:vlasov\_moments}
\int\_{}^{} \mathrm{d}^{3}v \left[ \frac{\partial f\_s}{\partial t} + \mathbf{v} \boldsymbol{\cdot} \frac{\partial f\_s}{\partial \mathbf{x}} + \frac{q\_s}{m\_s} \left( \mathbf{E} + \mathbf{v} \times \mathbf{B} \right) \boldsymbol{\cdot} \frac{\partial f\_s}{\partial \mathbf{v}} \right] \mathbf{v}^{k} = 0 \hspace{0.4cm}.
\end{equation}

Using the moments of the distribution function, \\(f\_{s}(\mathbf{x}, t)\\) can be replaced with the macroscopic variables to arrive at a fluid picture. Taking the zeroth moment (\\(k=0\\)) of Vlasov's equation yields a continuity equation, which states that in order to increase (decrease) the number density, a source (sink) is required to transport mass to (from) the respective location:

\begin{equation}
\label{eq:vlasov\_moments\_zero}
\frac{\partial n\_s}{\partial t} + \nabla \boldsymbol{\cdot} (n\_s \, \mathbf{u}\_s) = 0 \hspace{0.4cm}.
\end{equation}

This expression for mass continuity contains four unknowns, including the first moment of the distribution function \\(\mathbf{u}\_s\\), and therefore requires higher order moments to solve for \\(\mathbf{u}\_s\\). Evaluating the first moment (\\(k=1\\)) of Vlasov's equation and combining it with the continuity equation gives the equation of motion for each species and provides a differential expression for the bulk velocity:

\begin{equation}
\label{eq:vlasov\_moments\_one}
n\_s \, \frac{\mathrm{d} \mathbf{u}\_s}{\mathrm{d}t} = \frac{-\nabla \boldsymbol{\cdot} \boldsymbol{P\_s}}{m\_s} + \frac{n\_s q\_s}{m\_s} \left[ \mathbf{E}+\mathbf{u}\_s \boldsymbol{\times} \mathbf{B} \right] \hspace{0.4cm}.
\end{equation}

The resulting relation, known as the equation of motion in its non-conservative form, notably contains the second moment of the distribution function, the pressure tensor \\(\boldsymbol{P}\_s\\). In general, the \\(k^{th}\\) moment of Vlasov's equation contains the \\((k+1)^{th}\\) moment of the distribution function, and so closing the system inherently requires making an approximation for the highest order velocity moment retained. The plasma in Ganymede's interaction region is adiabatic to reasonable approximation, e.g., [Jia et al., 2008](https://agupubs.onlinelibrary.wiley.com/doi/10.1029/2007JA012748), conveniently permitting the adoption of a scalar pressure to close the system:
 
\begin{equation}
\label{eq:vlasov\_pressure}
P\_s = P\_{s,0} \left( \frac{n\_s}{n\_{s,0}} \right) ^{\kappa} \hspace{0.4cm}.
\end{equation}

In this relation, \\(\kappa=\frac{f+2}{f}\\), where \\(f\\) is the degrees of freedom. In a vacuum with no fields, this value would be set to \\(\kappa=\frac{5}{3}\\) for a monatomic plasma species or electrons, where \\(f=3\\) corresponds to three translational degrees of freedom. However, this value is typically set to \\(\kappa=2\\) in order to reflect the reduced degrees of freedom (\\(f=2\\)) imposed by the magnetic field \\(\mathbf{B}\\). Additionally, \\(n\_{s,0}\\) and \\(P\_0\\) are the empirically determined ``background" pressure and number density, respectively, for the undisturbed plasma. 

The Adaptive Ion Kinetic Electron Fluid (AIKEF) Hybrid Model
======

__Motivations for hybrid model__

At Ganymede, ion gyroradii in the Jovian magnetospheric background field are at most about 0.2 \\(R\_G\\), which is small compared to the scale of the interaction region (10s of Ganymede radii, e.g., see [Jia and Kivelson, 2021](https://agupubs.onlinelibrary.wiley.com/doi/abs/10.1002/9781119815624.ch35)). Because the gyroradius of a charged particle is proportional to its mass, electron gyroradii are at least three orders of magnitude smaller than for ions under similar conditions. As a result, models that employ a fluid treatment for both ions and electrons have produced reasonable approximations of Ganymede's global magnetospheric structure and plasma interaction (e.g., [Duling et al., 2014](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2022GL101688)). However, the magnetic field magnitude near Ganymede can fall to just a few nT in several expansive regions within its mini-magnetosphere (e.g., near the upstream magnetopause, where the magnetic fields of Ganymede and Jupiter are antiparallel and approximately equal in strength). Ions in these regions can possess gyroradii that are comparable in size to the interaction region and therefore require a fully kinetic treatment to resolve. On the other hand, electron gyroradii remain small and thus conducive to a fluid approach. A hybrid model (kinetic ions, fluid electrons) may therefore be required to fully characterize the plasma dynamics of Ganymede's interaction with the Jovian magnetosphere.

\begin{equation}
\label{eq:aikef\_a}
\frac{\mathrm{d}\mathbf{x}\_j}{\mathrm{d}t}=\mathbf{v}\_j \hspace{0.5cm} \mathrm{and} \hspace{0.5cm} \frac{\mathrm{d}\mathbf{v}\_j}{\mathrm{d}t}=\frac{e}{m\_j}(\mathbf{E}+\mathbf{v}\_j \times \mathbf{B})
\end{equation}

\begin{equation}
\label{eq:aikef\_b}
n\_e m\_e \frac{\mathrm{d}\mathbf{u}\_e}{\mathrm{d}t} = e n\_e (\mathbf{E}+\mathbf{u}\_e \times \mathbf{B}) - \nabla P\_e = 0
\end{equation}

\begin{equation}
\label{eq:aikef\_c}
\mathbf{E} = -\mathbf{u}\_i \times \mathbf{B} + \frac{(\nabla \times \mathbf{B}) \times \mathbf{B}}{\mu\_0 e n\_i } - \frac{\nabla P\_e}{e n\_i}
\end{equation}

\begin{equation}
\label{eq:aikef\_d}
\frac{\partial \mathbf{B}}{\partial t} = \nabla \times (\mathbf{u}\_i \times \mathbf{B}) - \nabla \times \left[ \frac{(\nabla \times \mathbf{B}) \times \mathbf{B}}{\mu\_0 e n\_i} \right]
\end{equation}

\begin{equation}
\label{eq:aikef\_e}
P\_e = P\_{e,0} \left( \frac{\rho\_\mathrm{C}}{\rho\_{\mathrm{C},0}} \right)^{\kappa}
\end{equation}

The model architecture I use to simulate plasma interactions within Jupiter's magnetosphere is the Adaptive Ion-Kinetic Electron-Fluid (AIKEF) hybrid model (see [Müller et al., 2011](https://www.sciencedirect.com/science/article/pii/S0010465510005266)). Rather than model each ion individually, AIKEF represents ions as macroparticles and electrons as a massless (i.e., the electron mass is set to zero), charge-neutralizing fluid. The dynamical system governing the motion of the plasma is given by the five expressions above, \ref{eq:aikef\_a} to \ref{eq:aikef\_e}, where ions are assumed to be singly charged with elementary charge \\(e\\). 

The ion macroparticles evolve in time according to the Lorentz force given in \ref{eq:aikef\_a}, where the \\(j\\) subscript indicates a particular macroparticle. Meanwhile, the motion of the electron fluid is governed by \ref{eq:vlasov\_moments\_one}, yielding \ref{eq:aikef\_b}. An adiabatic law is then assumed for the electron pressure \ref{eq:aikef\_e}. The electric field is calculated using the generalized Ohm's law displayed in \ref{eq:aikef\_c}. The ion species is denoted with the \\(i\\) subscript. This relation is derived by combining the continuity equation \ref{eq:vlasov\_moments\_zero} and the first moment of Vlasov's equation \ref{eq:vlasov\_moments\_one} for both the positively charged (ions) and negatively charged (electrons) species, and assumes that \\(\mathbf{u} \approx \mathbf{u}\_i\\). Additionally, Ampère's law is used to replace the electric current with the curl of the magnetic field (middle term on right-hand side of \ref{eq:aikef_c}), where the displacement current is neglected by applying the Darwin approximation. In the generalized Ohm's law, the term \\((-\mathbf{u}\_i \times \mathbf{B})\\) typically dominates the behavior of the electric field \\(\mathbf{E}\\) within the plasma. Indeed, "ideal" magnetohydrodynamic models (i.e., entire plasma represented by a single fluid) use only this term to describe the electric field but still accurately capture magnetospheric physics when the species comprising the plasma behave approximately synchronously at scales of interest (e.g., [Jia et al., 2008](https://agupubs.onlinelibrary.wiley.com/doi/10.1029/2007JA012748) and [Duling et al., 2014](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2022GL101688). The central and right-most terms on the right-hand side of \ref{eq:aikef_c} represent the Hall effect and the gradient of the electron pressure, respectively. The time evolution of the magnetic field \\(\mathbf{B}\\) is then determined by Faraday's law in \ref{eq:aikef_d}. 

__Benefits of a Hybrid Approach__

AIKEF's hybrid approach permits the resolution of different flow velocities for different ion species, which is critical for capturing how each species propagates through Ganymede's interaction region. This is especially pertinent when modeling ions with mass-to-charge ratios that differ by over an order of magnitude, as is the case between the dominant light and heavy constituents of  Ganymede's ionosphere (i.e., H\\(\_2^+\\) and O\\(\_2^+\\) at 2 amu and 32 amu, respectively). Additionally, whereas magnetohydrodynamic and multi-fluid treatments must assume a distribution function (e.g., Maxwellian, kappa distributions) for each ion species, AIKEF makes no assumptions about how the ions are distributed in velocity space. In their recent study, [Fatemi et al., 2022](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1029/2021JA029863) found that ions in the vicinity of Ganymede's upstream magnetopause are highly non-Maxwellian, thereby making this an important consideration when modeling the moon's plasma environment. By correctly describing the gyromotion of each ion species, AIKEF can also resolve suggested global asymmetries in plasma properties (e.g., see [Toth et al., 2016](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1002/2015JA021997)).