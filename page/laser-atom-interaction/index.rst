.. title: laser-atom interaction
.. slug: laser-atom-interaction
.. date: 2021-01-04 12:34:35 UTC-05:00
.. tags: Science, Physics, 
.. category: Research
.. link: 
.. description: 
.. type: text
.. status: featured
.. has_math: true

.. image:: /images/ionization.jpg
    :width: 400
    :align: left


LASER stands for Light Amplification by Stimulated Emition of Radiation.
Atoms and molecuels are the matter that is stimulated.  When a collection
of atoms is placed between two highly-reflected mirrors, the photons 
(pieces of light) bounce back and fourth further stimulating light into
a coherent (laser) beam.

Atoms are used to generate lasers, and lasers perturb the electrons surrounding atoms.
Atomic Molecular and Optical physicists study these interactions in depth to learn more
about nature, and to develop better lasers. 
During my graduate work I join two computational scientist at `Oak Ridge National Laboratory`_,
one was intrested in modeling quantum mechanical phenomena like laser-atom interactions
and the other was interested in creating a framework to empower scientists to take advantage
of distributed computing technology (aka supercomputers).

My research was based on the MADNESS_ framework written in C ++ which synthesizes a collection of
technologies to empower average scientists to leverage High Performance Computing
(HPC) in their research. In addition to hiding OpenMP and MPI details, MADNESS_ implements
a task-queue that frees users from tying their problem to specific numbers of processors. 

I wrote my thesis_ to be the guide for those interested in strong laser-atom interactions.
In it I discus my project's motivation, a review of competing algorithms for non-perturbative,
laser-atom interactions, published results, and potential future directions one could take
the project.

My `Physical Review A`_ publication was a proof of concept which accomplished the following:

- performed convergence studies to ensure self-consistency,
- reproduced previously published results,
- replicated an experimental pulse,
- predicted an array of two-photon ionization effects,
- demonstrated the flexibility of our code by modeling a molecular hydrogen ion \\(H_2^+\\)
  that included the internuclear coordinate on the same computational footing as the electron.

.. _thesis: https://www.amazon.com/Laser-atom-interactions-multiresolution-Nicholas-Vence/dp/3639713575
.. _MADNESS: /madness
.. _`Oak Ridge National Laboratory`: https://www.ornl.gov/
.. _`Physical Review A`: https://journals.aps.org/pra/abstract/10.1103/PhysRevA.85.033403
