## Applications of Imaginary Time Propagation Method in Material Research


We are seeking approximate solutions to the time-independent Schr&ouml;dinger equation

\begin{align*}
H\Psi_i&=E_i\Psi_i\tag{eqn. 1}\\
\text{with }\hat{H}=\hat T + \hat V &= -\frac{\hbar^2}{2m}\Delta + V
\end{align*}

If we can find the eigenfunctions that satisfy these equations, the corresponding eigenvalues can be found by calculating the expectation value

$$\langle \psi_i \rvert H \rvert \psi_i \rangle$$

Our approach is the solve the time-dependent Schr&ouml;dinger equation 

$$i\hbar\frac{\partial}{\partial t}\Psi = \hat H \Psi$$

by converting it via a Wick rotation (let $t=-i\tau$) to a simple heat equation:

\begin{align*}
\frac{\partial}{\partial t}\Psi=-\frac{\hat H}{\hbar}\Psi\tag{eqn. 2}
\end{align*}

Our solutions are then given by 

\begin{align*}\Psi(r,t)=e^{-\hat H\tau/\hbar}\psi(r,0)=\left(e^{-\hat H\Delta\tau/\hbar}\right)^n\psi(r,0)\tag{eqn. 3}\end{align*}

Then $\left(e^{-\hat H\Delta\tau/\hbar}\right)^n$ has the same eigenfunctions as $H\Psi_i=E_i\Psi_i$. If we iterate the decay equation it will converge on the ground state. 

We can solve eqn. 3 by treating it as an analog of the power method or its generalization the subspace iteration. 

As presented at NSF PREM Colloquium.

Helium droplets not only provide a unique matrix environment for high resolution spec- troscopy and studying molecular solvation but also allow to use guest molecules as probes of the surrounding quantum medium.1–3 After the initial discovery of the helium droplet tech- nique for spectroscopic applications, attention quickly turned into characterizing the physical properties of the helium droplets themselves. The groundbreaking experiments by the Toen-nies group employed the glyoxal molecule as a probe to study the helium droplet response through optical absorption spectrum.