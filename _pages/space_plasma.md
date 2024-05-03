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
 
To begin, we can characterize the behavior of space plasma in our solar system through the time evolution of the particle distribution function in a six-dimensional phase space \\(f_s(\mathbf{x},\mathbf{v},t)\\), where \\(f_s\\) is the average particle number density of species \\(s\\) with velocity \\(\mathbf{v}\\) at position \\(\mathbf{x}\\) and time \\(t\\). Starting from \\(f_s(\mathbf{x},\mathbf{v},t)\\) and assuming that no particles are created or destroyed (\\(\frac{\mathrm{d}f_s}{\mathrm{d}t}=0\\)), a straightforward application of the Lorentz force and Newton's second law yields the collision-free form of Boltzmann's equation, also known as Vlasov's equation (for further reference, see "Space Plasma Physics" by Baumjohann and Treumann, 2022):

\begin{equation}
\label{eq:vlasov}
\frac{\partial f_s}{\partial t} + \mathbf{v} \boldsymbol{\cdot} \frac{\partial f_s}{\partial \mathbf{x}} + \frac{q_s}{m_s} \left( \mathbf{E} + \mathbf{v} \times \mathbf{B} \right) \boldsymbol{\cdot} \frac{\partial f_s}{\partial \mathbf{v}} = 0 \hspace{0.4cm} .
\end{equation}

In this expression, \\(q_s\\) is the particle charge, \\(m_s\\) is the particle mass, \\(\mathbf{E}\\) is the electric field, and \\(\mathbf{B}\\) is the magnetic field. 

# Reducing Vlasov's equation to multi-fluid approach
While Vlasov's equation describes the dynamics of a non-collisional plasma population, this fully kinetic treatment is computationally very expensive at most scales of interest (including for moon-magnetosphere interactions). Indeed, this approach has never been used by itself to create a three-dimensional model of the interaction between a moon and its plasma environment. Rather than solving the characteristics of Vlasov's equation directly, we can instead adopt a fluid picture in order to reduce the degrees of freedom inherent to the kinetic approach. Moreover, if the characteristic scale of space and time of the plasma environment are large compared to the gyroradii and inverse gyrofrequency of a specific particle species, respectively, we can approximate Vlasov's equation without truncating any meaningful dynamics. One method is the multi-fluid approach, where each species \\(s\\) is prescribed a fluid treatment using a limited set of macroscopic quantities that are independent of individual particle velocity \\(\mathbf{v}\\).

# The velocity moments of distribution function
To generate these variables, we can integrate the product of the distribution function of each plasma species \\(f_s\\) with the velocity \\(\mathbf{v}^{k}\\) raised to integer powers of \\(k\\) over all of the three-dimensional velocity space:

\begin{equation}
\label{eq:moments}
M_{k,s} = \int_{}^{} \mathrm{d}^{3}v \; f_s(\mathbf{x}, \mathbf{v}, t) \; \mathbf{v}^{k}\hspace{0.4cm} .
\end{equation}

In this expression, \\(M_{k,s}\\) is the \\(k^{\mathrm{th}}\\) statistical velocity moment of the distribution function for species \\(s\\). The "zeroth" moment (\\(k=0\\)) is thus the simple integral of the distribution function over all of velocity space, which intuitively yields the number density \\(n_{s}(\mathbf{x},t)\\) of species \\(s\\) at location \\(\mathbf{x}\\) and time \\(t\\). Evaluation of the first order moment \\(M_{1,s}\\) is similarly straightforward and gives the product of the number density and the average velocity, or "bulk" plasma flow \\(\mathbf{u}_{s}(\mathbf{x},t)\\):

\begin{equation}
\label{eq:zeroth_mom}
M_{0,s} = n_{s}(\mathbf{x},t) = \int_{}^{} \mathrm{d}^{3}v \; f_s(\mathbf{x}, \mathbf{v}, t)
\end{equation}

\begin{equation}
\label{eq:first_mom}
M_{1,s} = n_{s}(\mathbf{x},t) \mathbf{u}_{s}(\mathbf{x},t) = \int_{}^{} \mathrm{d}^{3}v \; f_s(\mathbf{x}, \mathbf{v}, t) \; \mathbf{v} \hspace{0.4cm} .
\end{equation}

From these two macroscopic quantities, we can also define another useful physical parameter, the current density \\(\mathbf{j}(\mathbf{x},t)\\):

\begin{equation}
\label{eq:current}
\mathbf{j}(\mathbf{x},t) = \sum_{s} \; \mathbf{j}_s = \sum_{s} \; q_{s} \; n_{s}(\mathbf{x},t) \; \mathbf{u}_{s}(\mathbf{x},t) \hspace{0.4cm} .
\end{equation}

The expressions for higher order moments of the distribution function (\\(k \geq 2\\)) are not singularly defined. For example, when solving the second order moment (\\(k=2\\)), the term \\(\mathbf{v}^{2}\\) can represented as either \\((\mathbf{v} \boldsymbol{\cdot} \mathbf{v})\\) or \\((\mathbf{v} \boldsymbol{\otimes} \mathbf{v})\\), where (\\(\boldsymbol{\cdot}\\)) and (\\(\boldsymbol{\otimes}\\)) correspond to the inner (dot) and outer (Kronecker or tensor) products, respectively. Applying the inner product and substituting \\((\mathbf{v}-\mathbf{u}_{s})\\) for the velocity provides the "kinetic" temperature, a measure of the spread of the distribution function in velocity space about the average velocity \\(\mathbf{u}_s\\). An analogous treatment for the outer product case yields the pressure tensor (the mass \\(m_s\\) of species \\(s\\) is required in this expression to obtain the correct units of pressure):

\begin{equation}
\label{eq:secondMom}
M_{2,s} = \boldsymbol{P}_{s}(\mathbf{x},t) = m_s \int_{}^{} \mathrm{d}^{3}v \; f_s(\mathbf{x}, \mathbf{v}, t) \; (\mathbf{v}-\mathbf{u}_s) \boldsymbol{\otimes} (\mathbf{v}-\mathbf{u}_s) \hspace{0.4cm} .
\end{equation}

Revisiting Vlasov's equation (\ref{eq:vlasov}) equipped with the relationship between these macroscopic quantities and the distribution function, we can now construct a single fluid (magnetohydrodynamic) or multi-fluid picture for characterizing plasma dynamics. As with the distribution function, the product of Vlasov's equation and velocity \\(\mathbf{v}^k\\) raised to integer powers of \\(k\\) can be integrated over velocity space to obtain equations of state that are independent of the velocity:

\begin{equation}
\label{eq:vlasovMoments}
\int_{}^{} \mathrm{d}^{3}v \left[ \frac{\partial f_s}{\partial t} + \mathbf{v} \boldsymbol{\cdot} \frac{\partial f_s}{\partial \mathbf{x}} + \frac{q_s}{m_s} \left( \mathbf{E} + \mathbf{v} \times \mathbf{B} \right) \boldsymbol{\cdot} \frac{\partial f_s}{\partial \mathbf{v}} \right] \mathbf{v}^{k} = 0 \hspace{0.4cm} .
\end{equation}

Using the moments of the distribution function, \\(f_{s}(\mathbf{x}, t)\\) can be replaced with the macroscopic variables to arrive at a fluid picture. Taking the zeroth moment (\\(k=0\\)) of Vlasov's equation yields a continuity equation, which states that in order to increase (decrease) the number density, a source (sink) is required to transport mass to (from) the respective location:

\begin{equation}
\label{eq:continuity}
\frac{\partial n_s}{\partial t} + \nabla \boldsymbol{\cdot} (n_s \, \mathbf{u}_s) = 0 \hspace{0.4cm} .
\end{equation}

This expression for mass continuity contains four unknowns, including the first moment of the distribution function \\(\mathbf{u}_s\\), and therefore requires higher order moments to solve for \\(\mathbf{u}_s\\). Evaluating the first moment (\\(k=1\\)) of \ref{eq:vlasov} and combining it with the continuity equation gives the equation of motion for each species and provides a differential expression for the bulk velocity:

\begin{equation}
\label{eq:vlasov_zeroth}
n_s \, \frac{\mathrm{d} \mathbf{u}_s}{\mathrm{d}t} = \frac{-\nabla \boldsymbol{\cdot} \boldsymbol{P_s}}{m_s} + \frac{n_s q_s}{m_s} \left[ \mathbf{E}+\mathbf{u}_s \boldsymbol{\times} \mathbf{B} \right] \hspace{0.4cm} .
\end{equation}

The resulting relation, known as the equation of motion in its non-conservative form, notably contains the second moment of the distribution function, the pressure tensor \\(\boldsymbol{P}_s\\). In general, the \\(k^{th}\\) moment of Vlasov's equation contains the \\((k+1)^{th}\\) moment of the distribution function, and so closing the system inherently requires making an approximation for the highest order velocity moment retained. The plasma in Ganymede's interaction region is adiabatic to reasonable approximation, e.g., (Jia et al. 2008)[https://agupubs.onlinelibrary.wiley.com/doi/10.1029/2007JA012748], conveniently permitting the adoption of a scalar pressure to close the system:
 
\begin{equation}
\label{eq:pressure}
P_s = P_{s,0} \left( \frac{n_s}{n_{s,0}} \right) ^{\kappa} \hspace{0.4cm}
\end{equation}

In this relation, \\(\kappa=\frac{f+2}{f}\\), where \\(f\\) is the degrees of freedom. In a vacuum with no fields, this value would be set to \\(\kappa=\frac{5}{3}\\) for a monatomic plasma species or electrons, where \\(f=3\\) corresponds to three translational degrees of freedom. However, this value is typically set to \\(\kappa=2\\) in order to reflect the reduced degrees of freedom (\\(f=2\\)) imposed by the magnetic field \\(\mathbf{B}\\). Additionally, \\(n_{s,0}\\) and \\(P_0\\) are the empirically determined ``background" pressure and number density, respectively, for the undisturbed plasma. 

The Adaptive Ion Kinetic Electron Fluid (AIKEF) Hybrid Model
======

__Motivations for hybrid model__

At Ganymede, ion gyroradii in the Jovian magnetospheric background field are at most about 0.2 \\(R_G\\), which is small compared to the scale of the interaction region (10s of Ganymede radii, e.g., see \cite{Jia2021}). Because the gyroradius of a charged particle is proportional to its mass, electron gyroradii are at least three orders of magnitude smaller than for ions under similar conditions. As a result, models that employ a fluid treatment for both ions and electrons have produced reasonable approximations of Ganymede's global magnetospheric structure and plasma interaction (e.g., Duling et al., 2014). However, the magnetic field magnitude near Ganymede can fall to just a few nT in several localized regions (e.g., near the upstream magnetopause, where the magnetic fields of Ganymede and Jupiter are antiparallel and approximately equal in strength). Ions in these regions can possess gyroradii that are comparable in size to the interaction region and therefore require kinetic treatment to resolve. On the other hand, electron gyroradii remain small and thus conducive to a fluid approach. A hybrid model (kinetic ions, fluid electrons) may therefore be required to fully characterize the plasma dynamics of Ganymede's interaction with the Jovian magnetosphere.