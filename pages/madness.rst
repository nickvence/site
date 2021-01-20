.. title: MADNESS
.. slug: madness
.. date: 2021-01-15 16:30:56 UTC-05:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: true

------------
Introduction
------------

.. figure:: /images/3Dmesh.jpg
    :alt: Different 3D mesh resolutions.
    :align: right
    :width: 450
    :figwidth: 450

    **Fig. 1** A finite element grid of a three dimensional problem domain for 
    slightly different resolutions have a vastly different number of elements.
    
Partial Differential Equations (PDE) are used to approximate solutions to a variety of
real world problems.
MADNESS (Multiresolution ADaptive Numerical Environment for Scientific Simulation) is
a framework written in C++ for solving differential (and integral) equations efficiently
on High Performance Compouters (HPC).

A fist step to learning MADNESS, is understanding the most popular model for solving PDSs:
the `finite element method`_.
The finite element scheme discretizes space (divide it into boxes) and then
creates functions and opperators to approximate solution on the mesh (the discretized space).
While this popular approach works for a wide variety of problems, it breaks down for
high-dimension problems that requre scalable resoltion.

.. table:: Memory as a function of :math:`N_{size}`.
    :class: float-left

    +----------------+----------------+
    | \\[N_{side}\\] | Memory         |
    +----------------+----------------+
    | 4              | 64             |
    +----------------+----------------+
    | 6              | 216            |
    +----------------+----------------+
    | 8              | 512            |
    +----------------+----------------+
    | 10             | 1000           |
    +----------------+----------------+

As a practical example, suppose you are interested in modeling water waves on a
three dimensional finite element grid.
Now suppose you need to include the interaction large ocean waves (large simulation
volume) and the small waves of an outboard motor (higher resolution).
Having lot of points (high resolution) over a large volume consumes lots of memory.
This algorithm will also scale poorly when you need to increase the resolution:
increase :math:`N_{size}`, for 

\\[\\text{Memory size} = O(N_{side}^3) \\].

This is an example of a problem that spans multiple length-scales, and it is the
Achilles heel of the finite element method.

Matt Reuter was interested in modeling the electric field between the tip of an
`atomic force microscope`_ (AFM) and the substrate while examining its effect on
molecule caught in the middle.  To visualize how an AFM works, imagine a pencil
penetrating the far end of aa thin flexible 12 inch ruler at a right angle, while
you hold the near end. As you drag this pencil-lever across a surface the pencil-end
of the ruler goes up and down depending on the height of the surface.  Now imagine
a laser (bounced off a mirror fixed to the far end of the ruler) is used to measure
the height. While a real AFM uses small precision parts, this is the first half
of how an AFM works. For an amazingly-cool video description of AFM, watch the
following from  `Quantum Made Simple`_

.. youtube:: 8gCf1sEn0UU

This second half of how AFM works is there is an electric field between the
"pencil tip" and the surface (substrate) to increase the AFM sensitivity.

How did Matt model this electric field (gradient) on a large scale, and the 
electric-molecular coupling on sub-nano scale?  He used MADNESS.

----------------------------------------
MADness: a Multiresolution ADaptive mesh
----------------------------------------

.. figure:: /images/madness1.png
    :alt: A multiresolution mesh
    :align: right
    :width: 300
    :figwidth: 350

    **Fig. 2** A multiresolution mesh has more than one size box in the grid.
    An adaptive grid places extra grid points just where they are needed.

One way of solving the issue of multiple length-scales on a high-dimentional 
domain is to introduce a Multiresolution (variable-size) mesh
shown in Fig. 2.  Multiresolution is the M in the MADNESS acronym.
In the AFM example, the shape of the electric field gradient is important, but
it takes up a large simulation volume while the details of the electronic structure of the molecule 
requires a lot of resolution (boxes).

Grid point placement is problem on a multiresolution grid, is where to place the extra 
grid points.  Will small ripples in your function stay in one place or will they
move over time?  To solve this issue, MADNESS uses an ADaptive (mADness) mesh:
one that changes to meet the needs of the function being modeled. If you're
interested in the mathematical details of adaptive refinement, see Section 2.1
of our `SIAM Publication`_.

----------------------------------------------------------
madNESS: a Numerical Environment for Scientific Simulation
----------------------------------------------------------

This numerical environment for scientific simulation (madNESS) consists of the following
features:

- The illusion of Basis-free computing.
- Not having to choose between a computationally-efficient, non-physical, C++ function
  or a computationally-inefficient physical-meaningful C++ function.
- A parallel runtime environment engineered for load balancing hundreds of thousands of cores.
- An API for representing function and operators optimized for scaling to large
  numbers of processors.

"Basis-free" computing
----------------------

When modeling physical processes (electron wave function, electrostatic potential,
fluid flow, etc.), one can take advantage of the solution's structure to choose
an efficient basis for computation: sine waves for example.  For certain classes
of problems this sparse basis delivers a high performance solution. However, as
one pushes the limits of these solution classes, the sparse basis often fails to
capture interesting artifacts.
For this reason the finite element basis is seen as the (computationally expensive)
brute force aproach sure to capture the artifacts present.

Efficient physical functions
----------------------------

In the early days of distributed computing it was not uncommon to hard-code the
computational architecture into the code. For instance, having 64 CPUs, one
might choose a spatial grid that was 4x4x4 (see Fig. 1).  Thus, the spatial
resolution of his simulation was directly tied to his computational architecture
making it difficult to scale to larger number of processors.

Scientits are often frozen out of advancment when their code isn't able to scale
to take advantage of continued computational growth. This usually the result of
professionals working outside of their area of expertice.  For instance in 
other computational frameworks, one often has to work with computational objects
that do not map onto the physical 
force the programer to think in computational (rather than physical) abstractions.
MADNESS's 

Creating code that can take advantage of modern supercomputers can be a career.
Unfortunately, most scientists already have a career, and can't afford to put it
on hold while learning the hoops of distributed computing.  
In the spirit of standing on the shoulders of giants, here is a list of 
of technologies that MADNESS uses to allowing average programmers to "see further."

- **Global Arrays** an API for shared memory programming on distributed memory computers.
- **MPI** Message Passing Interface for distributed-memory parallel programming.
- **OpenMP** an interface to shared-memory parallel programming.
- **Pthreads** an thread execution model governing non-concurrent bundles of computation.
- **Futures** a C++ standard providing synchronization across threads.


References
----------

The `SIAM publication`_ is a scholarly introduction to the mathematics of MADNESS
aloging with its swuit of computational technologies and applications.

The `Physics Review A Publication`_ is my introduction to MADNESS written to the
atomic physics community. 

Here is a brisk, 15-minute overview of MADNESS from its founder Robert Harrison.

.. youtube:: dBwWjmf5Tic

.. _`finite element method`: https://en.wikipedia.org/wiki/Finite_element_method
.. _`atomic force microscope`: https://en.wikipedia.org/wiki/Atomic_force_microscopy
.. _`Quantum Made Simple`: http://toutestquantique.fr/en/afm/
.. _`SIAM Publication`: https://amath.colorado.edu/faculty/beylkin/papers/H-B-B-C-F-F-G-etc-2016.pdf
.. _`Physics Review A Publication`: https://journals.aps.org/pra/abstract/10.1103/PhysRevA.85.033403
