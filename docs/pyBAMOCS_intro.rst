============
Introduction
============

PyBAMOCS uses a simplified box model to simulate the Atlantic meridional
overturning circulation (AMOC) simulation, used by Gnanadesikan et al.1
to examine the stability of the AMOC. This code facilitates
experimenting with the box model, including:

-  Documented API for running individual box model simulations
-  Generating data from many box model simulations
-  Storing box model data in NetCDF format

License
-------

Copyright 2022, The Johns Hopkins University Applied Physics Laboratory
LLC

All rights reserved.

Distributed under the terms of the BSD 3-Clause License.

Requirements
------------

This code is tested on ``Python version 3.9``. The box model code also
leverages the following packages: - ``numpy`` - ``seawater`` -
``matplotlib`` - ``netCDF4`` - ``pytest`` (for running unit tests)

Getting Started
---------------

Python
~~~~~~

If you are unfamiliar with Python, we recommend downloading Anaconda
from `here <https://www.anaconda.com/products/individual>`__. Follow the
instructions to install Anaconda, and then create a new conda
environment (or virtual environment if not using Anaconda) by running

.. code:: commandline

   $ conda create -n <environment name> python==3.9

Follow the prompts to finish creating the new environment. Once
finished, activate the environment via the following command:

.. code:: commandline

   $ conda activate <environment name>

The name of your environment should be in front of your command line,
for example if you named your environment “myenv”, it should appear as
follows:

.. code:: commandline

   (myenv) $

Running the code
~~~~~~~~~~~~~~~~

Assuming you have the code pulled from the repository, navigate to the
``pybamocs`` directory and run

.. code:: commandline

   $ pip install -e .

This should install the required packages to run the Box model code in
your Python environment. Once installed, you should be able to run the
follow scripts from the `scripts <pybamocs/scripts>`__ directory:

-  `simple_example.py <pybamocs/scripts/simple_example.py>`__: An
   example of how to run a single box model simulation. Also creates
   plots of the results.
-  `collapse_example.py <pybamocs/scripts/collapse_example.py>`__:
   Several examples of AMOC collapse with plotted results.
-  `store_example_npz.py <pybamocs/scripts/store_example_npz.py>`__: An
   example of how one might save box model data as Numpy ``.npz`` files.
-  `store_example_netcdf.py <pybamocs/scripts/store_example_netcdf.py>`__:
   An example of how one might save box model data in NetCDF format.
-  `datagen_simulation.py <pybamocs/scripts/datagen_simulation.py>`__: A
   script simulating a parameter space exploration experiment using
   small random perturbations on box model variables. Each simulation
   result is stored in NetCDF format.

Box model example
~~~~~~~~~~~~~~~~~

The very basic example of running the box model can be illustrated in
the following example:

.. code:: python

   from pybamocs.box_model import box_model
   from pybamocs.box_model_args import (
       BoxModelBoxDimensions,
       BoxModelInitConditions,
       BoxModelParameters,
       BoxModelTimeStep
   )

   # Define box model arguments (see definitions of each argument for available parameters and default settings):
   box_dimensions = BoxModelBoxDimensions()
   box_initial_conditions = BoxModelInitConditions()
   box_parameters = BoxModelParameters(M_ek=35e6)
   box_time_step = BoxModelTimeStep(n_steps=100)

   # the box model returns a BoxModelResult object
   result = box_model(box_dimensions, box_initial_conditions, box_parameters, box_time_step)

   # Call the `unpack` method to get the various result values (if desired):
   M_n, M_upw, M_eddy, D_low, T, S, sigma0 = result.unpack()

Organization
------------

::

   python
   │   scripts - Location for local code examples and other useful scripts
   │   notebooks - Location for Jupyter Notebooks
   |   setup.py - Installation script for module (typically associated with `pip install`)
   |   README.md - This file
   │   tests - Location for test scripts
   └───pybamocs - Top level Python module
       └───test - Location of test scripts
           └───data - Directory of files used for test
       |   box_model.py - box model code
       |   box_model_args.py - box model box model argument objects
       |   box_storage.py - functions for storing box model data to disk
       |   constants.py - various constants used between box model files

For Developers
--------------

When contributing to this code, we ask: - Start by creating a branch of
the repository, then writing the code, and creating a merge request -
Consider creating an issue on GitHub for more in-depth or challenging
additions or problems - Please use type hinting for arguments and
results - Remember to add the copyright statement at the top of the file
- Add unit tests where feasible (using ``pytest``) to the ``test``
directory, and briefly describe the tests below

Running the tests
~~~~~~~~~~~~~~~~~

As previously mentioned, we currently use ``pytest`` for the tests we
have. To run the tests, simply navigate to the ``python`` directory on
the command line and run:

.. code:: commandline

   pytest

and the tests will run. Current testing includes the following:

-  Compare with data produced by the original Matlab version of the box
   model and ensure the differences is values is small
-  Compare the box model results using the default parameters with
   previously generated data

Acknowledgements
----------------

This code was produced under the DARPA AI-assisted Climate Tipping-point
Modeling (ACTM) program, and in association with Johns Hopkins
University.

1 Gnanadesikan, A., Kelson, R., & Sten, M. (2018). Flux Correction and
Overturning Stability: Insights from a Dynamical Box Model, Journal of
Climate, 31(22), 9335-9350. Retrieved Mar 30, 2022 from