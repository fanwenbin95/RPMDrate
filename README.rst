*************************************************************************
RPMDrate - Bimolecular reaction rates via ring polymer molecular dynamics
*************************************************************************

About RPMDrate
==============

RPMDrate is a free, open-source software package for using ring polymer
molecular dynamics simulations to compute properties of chemical systems,
including, but not limited to, chemical reaction rates.

License
=======

RPMDrate is distributed under the terms of the 
`MIT license <http://www.opensource.org/licenses/mit-license>`_::

    Copyright (c) 2012 by Joshua W. Allen (jwallen@mit.edu)
                          William H. Green (whgreen@mit.edu)
                          Yury V. Suleimanov (ysuleyma@mit.edu, ysuleyma@princeton.edu)
    
    Permission is hereby granted, free of charge, to any person obtaining a 
    copy of this software and associated documentation files (the "Software"), 
    to deal in the Software without restriction, including without limitation
    the rights to use, copy, modify, merge, publish, distribute, sublicense, 
    and/or sell copies of the Software, and to permit persons to whom the 
    Software is furnished to do so, subject to the following conditions:
    
    The above copyright notice and this permission notice shall be included in
    all copies or substantial portions of the Software.
    
    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL
    THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING 
    FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER 
    DEALINGS IN THE SOFTWARE. 

Installation
============

Dependencies
------------

RPMDrate depends on several other packages in order to provide its full
functional capabilities. The following dependencies are required to compile
and run RPMD:

* `Python <http://www.python.org/>`_ (version 2.5.x or later, including any version of Python 3, is recommended)

* `NumPy <http://numpy.scipy.org/>`_ (version 1.5.0 or later is recommended)

* `FFTW <http://www.fftw.org/>`_ (version 3.3 or later is recommended)

* A standard Fortran 90/95 compiler (e.g. ``gfortran``, ``g95``, ``ifort``, etc.)

RPMDrate uses the ``f2py`` tool (bundled with NumPy) to expose its Fortran code
to Python.

Getting RPMDrate
----------------

The best way to obtain a copy of the repository is to clone it using `git
<http://git-scm.com/>`_::

    $ git clone git://github.com/GreenGroup/RPMDrate.git

This enables you to easy update your local clone with the latest changes. If
you intend to contribute, you should fork the project on GitHub first, and
clone from your fork.

Compiling from Source
---------------------

RPMDrate is installed by invoking the ``setup.py`` script::

    $ python setup.py install

This will compile the Fortran source and Python wrapper code, which may take
some time. Note that you may need administrator privileges to install RPMD.

If you wish to use RPMDrate without installing, simply add the folder containing
this file to your ``PYTHONPATH`` environment variable and compile the source
code in-place using the command ::

    $ python setup.py build_ext --inplace

A Makefile that wraps these commands has been provided. The Makefile also
provides a clean target for deleting all files created during compilation.

Running the Unit Tests
----------------------

RPMDrate comes with a large suite of unit tests that ensure functionality is
working as intended. To run these tests, first install RPMDrate or compile it
from source in-place using the directions above. Then, simply invoke the entire
suite of unit tests using ::

    $ python setup.py test

This will run all of the unit tests in sequence, which may take some time. If
one or more unit tests fail, please report them on the GitHub issue tracker
(if they aren't already reported).


Modification
============

* **Bug fix**: zero variance would interrupt

* **Bug fix**: `r2r` (real to real) FFT does not work. Replace with common complex FFT. Then combine real and complex parts manually. 

* **Update**: In `main.py`, `:.4f` -> `:.8f`, `:.6f` -> `:.10f`. Especially, the reaction coordinate ($\xi$) would be written full.  

* **Modify**: max number of bead `Nmax`, 1024 -> 64. You should change this parameter in `math.f90` if you perform a calculation with more than 64 beads. 

* **Add**: `.xyz` format of umbrella configurations

* **Add**: free energy change (kcal/mol) in the rate file. 

* **Add**: Print last frame of each trajectory in xyz format, include bead `umbrella_sampling_{0:.8f}_bead.xyz` and centroid `umbrella_sampling_{0:.8f}_centroid.xyz`. The reaction coordinate, average energy, and centroid energy present in the title line. Coordinate is in Angstrom. In the bead trajectory file, the momentum (in atomic unit, scientific format `14.8e`) and the gradient (in kcal/mol per Angstrom, float format `14.8f`) present after the coordinate. In the centroid trajectory file, the gyration radius (in Angstrom) present after the coordinate. 

* **Add**: Print energy of last frame in each trajectory in `umbrella_sampling_{0:.8f}_traj_info.dat`. 
    * Column 1: evolved number of steps
    * Below items are in one trajectory. 
        * Column 2: sum of xi
        * Column 3: sum of xi squared
        * Column 4: average of xi
        * Column 5: variance of xi
    * Below items are average energy of the last frame in one trajectories in kcal/mol. 
        * Column 6: potential energy of centroid
        * Column 7: mean potential energy (Emean)
        * Column 8: kinetic energy (Ek)
        * Column 9: ring energy (Ering)
        * Column 10: total energy (Emean, Ek, Ering)
    * Next Nbeads columns are the energies of each copy / image / bead of the system. 