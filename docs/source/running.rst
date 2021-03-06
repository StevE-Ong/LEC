.. _input parameters:

Input parameters
================

.. admonition:: Note!

   New ``PARAM`` may be added from time to time.

This section describes the details of the input parameters, output files, and examples. The code was written in ``fortran90``. The input parameters are contained in the file ``input.dat`` in the folder ``Data``.

.. code-block:: fortran

   &PARAM1  jobno=001,ksmax=1000000,div=60.d0,div2=40.d0                      ,&END
   &PARAM2  SL=1.0d22,Ev=10.d6,pw=0.8d-6,pp=5.6d-6,sp=1.44d-6                 ,&END
   &PARAM3  alpha=0.0d-6,enum=1.0d9,bin=1.d6,shot=1,inc_ang = 0.d0            ,&END
   &PARAM4  xinit=1.1d0,rmass=1.d0,sigmax=0.01d0,sigmay=0.01d0,sigmaz=0.01d0  ,&END
   &PARAM5  iconR=2,QED=1,ipl=0,shape=0,OutRad=0,OutPairs=0                   ,&END
   &PARAM6  loadpar=1,loadseed=33587,qedseed=0                                ,&END

**jobno** [*Integer*] The job numbering.

**ksmax** [*Integer*] Maximum number of time step.

**div,div2** [*Float*] The number of division or cells per Lamor radius.

**SL** [*Float*] Laser intensity [:math:`\mathrm{W~cm^{-2}}`].

**Ev** [*Float*] Electron energy [eV].

**pw** [*Float*] Laser wavelength [meter].

**pp** [*Float*] Laser pulse length [meter]. Pulse duration is :math:`\tau_\mathrm{L}=\mathrm{pp}/c`. Pulse duration at FWHM is :math:`\tau_\mathrm{L}\times 1.1744`

**sp** [*Float*] Laser waist radius (at :math:`1/e^2`) [meter].

**alpha** [*Float*] Electron waist radius (at :math:`1/e^2`) [meter]. Set to 0.0d-6 for a single electron. If set to a value larger than 0, electron energy distribution in 1D and 2D will be outputted.

**enum** [*Float*] Electron number in a bunch. Used for radiation calculation. For single electron, ``enum=1.d0``

**bin** [*Float*] Radiation spectrum bin size [eV].

**shot** [*Integer*] Number of shot.

**inc_ang** [*Float*] Incident angle of the electron with respect to the x-axis [degree].

**xinit** [*Float*] Initial position of the electron from the collision point. The collision happens at :math:`t=0`. The laser is one pulse length away from :math:`t=0`.

**rmass** [*Float*] The charge to mass ratio of the colliding particle. Setting to ``rmass=206`` represents muon.

**sigma_{x,y,z}** [*Float*] Electron momentum energy spreads in three directions. This input is ignored for a single electron

**iconR** [*Integer*] Specifying the particle pusher used.

   * ``iconR=0`` Lorentz force
   * ``iconR=1`` Sokolov
   * ``iconR=2`` Reduced Landau-Liftshitz
   * ``iconR=3`` Stochastic

**QED** [*Integer*] Specifying whether to use QED for Sokolov and Reduced Landau-Liftshitz. ``QED=1`` is mandatory for Stochastic process.

**ipl** [*Integer*] Specifying laser polarisation.

   * ``ipl=0`` linear polarisation (LP)
   * ``ipl=1`` circular polarisation (CP).

**shape** [*Integer*] Laser spatial and temporal profile. Laser profile can be modified in *laser.f*.

   * ``shape=0`` 0th order Gaussian beam.
   * ``shape=1`` 5th order paraxial approximation.

.. admonition:: Note!

   The temporal profile for 5th order paraxial approximation is not Gaussian. The temporal profile is :math:`g=\mathrm{cosech}((t-x)/\tau_\mathrm{L})`.

**loadpar** [*Integer*] Load particle energy distribution from external file. The default filename is ``dist_fn.dat``. To change the filename, please edit the subroutine ``manual_load``.

**OutRad** [*Integer*] Specifying whether to calculate radiation. When setting ``OutRad=1``, emission spectrum, photon number distribution, radiation angular distribution will be calculated. This part consumes most of the simulation time.

**OutPairs** [*Integer*] Specifying whether to calculate pair production. The code currently support the Bethe-Heitler pair production. The cross section for Bremsstrahlung and pair production will be calculated if ``OutPairs=1``. The Z component of nucleus for the specific converter is specify in ``module random_commom``.

**loadseed**, [*Integer*] Specifying the user define seed for the loading of momentum distribution. If set to zero then the default seed in the code will be used.

**qedseed**, [*Integer*] Specifying the user define seed for Stochastic process (``iconR=3``). If set to zero then the default seed in the code will be used. To obtain different stochastic event of the same electron bunch, keep ``loadseed`` fixed and change ``qedseed``.

Output files
============

Example of a single electron for ``Lorentz vs Sokolov``. The input parameters are in *examples/Data1(2)*

.. code-block:: fortran

   &PARAM1  jobno=001,ksmax=10000000,div=128.d0,div2=40.d0	            ,&END
   &PARAM2  SL=1.0d22,Ev=100.d6,pw=0.8d-6,pp=5.6d-6,sp=1.44d-6              ,&END
   &PARAM3  alpha=0.0d0,enum=1.0d0,bin=1.d6,shot=1,inc_ang = 0.d0           ,&END
   &PARAM4  xinit=5.d0,rmass=1.d0,sigmax=0.01d0,sigmay=0.01d0,sigmaz=0.01d0 ,&END
   &PARAM5  iconR=0,QED=0,ipl=0,shape=0,load=0,OutRad=1,OutPairs=0 	    ,&END

.. code-block:: fortran

   &PARAM1  jobno=002,ksmax=10000000,div=128.d0,div2=40.d0	            ,&END
   &PARAM2  SL=1.0d22,Ev=100.d6,pw=0.8d-6,pp=5.6d-6,sp=1.44d-6              ,&END
   &PARAM3  alpha=0.0d0,enum=1.0d0,bin=1.d6,shot=1,inc_ang = 0.d0           ,&END
   &PARAM4  xinit=5.d0,rmass=1.d0,sigmax=0.01d0,sigmay=0.01d0,sigmaz=0.01d0 ,&END
   &PARAM5  iconR=1,QED=0,ipl=0,shape=0,load=0,OutRad=1,OutPairs=0 	    ,&END

.. admonition:: Note!

   New ``PARAM`` may be added from time to time. Please refers to the :ref:`input list <input parameters>` to see the lastest ``PARAM``.

The outputs are written in ASCII format. The file ``output001.dat`` records the detail parameters of the simulation. For example:

.. code-block:: fortran

   Parameters for pulse laser

   Laser polarization: linear

   0th order Gaussian beam

   Laser Intensity               [W/cm2]   1.0000000000000000E+022
   Peak electric field             [V/m]   274000000000000.00
   Peak magnetic field           [Gauss]   9280000000.0000000
   Larmor radius for light speed     [m]   1.8318965517241380E-009
   laser wavelength                  [m]   7.9999999999999996E-007
   pulse length                      [m]   5.5999999999999997E-006
   pulse duration                    [s]   1.8666666666666665E-014
   pulse duration (FWHM)             [s]   2.1978133333333330E-014
   waist radius (1/e2)               [m]   1.4400000000000000E-006

   parameters for electron beam
   ...

The file ``orbt1q001.dat`` records the trajectories, energy etc. of the particle. For a single electron, there are 7 files recoding the same output. For example:

+----------+-------+-------+--------------------------+--------------------------+---------------------+--------------------------------+
|    0     |   1   |   2   |		3   	      |		    4  	 	 |	   5           |	      6			|
+==========+=======+=======+==========================+==========================+=====================+================================+
| time [s] | x [m] | y [m] | :math:`p_x` [normalized] | :math:`p_y` [normalized] | kinetic energy [eV] | :math:`\chi_e` [dimensionless] |
+----------+-------+-------+--------------------------+--------------------------+---------------------+--------------------------------+


.. code-block:: fortran

   -0.466547E-13     0.279928E-04    -0.203395E-54    -0.196692E+03    -0.168832E-43     0.100511E+09     0.360270E-45
   -0.466428E-13     0.279857E-04    -0.166512E-53    -0.196692E+03    -0.696642E-43     0.100511E+09     0.753717E-45
   -0.466309E-13     0.279785E-04    -0.574687E-53    -0.196692E+03    -0.161358E-42     0.100511E+09     0.117740E-44
   -0.466190E-13     0.279714E-04    -0.139148E-52    -0.196692E+03    -0.294680E-42     0.100511E+09     0.162775E-44
   -0.466070E-13     0.279642E-04    -0.277271E-52    -0.196692E+03    -0.471967E-42     0.100511E+09     0.210042E-44
   -0.465951E-13     0.279571E-04    -0.488190E-52    -0.196692E+03    -0.695102E-42     0.100511E+09     0.259021E-44
   ...

.. admonition:: Note!

   The numbers **0**, **1**, **2**,...indicate the columns to extract the data by using **usecols=[0,1,2,...]** in :ref:`Python <python>`.
   For :ref:`gnuplot <gnu>`, the columns number becomes **($1)**, **($2)**, **($3)**,...

The file ``phtne001.dat`` records the radiation output. For example:

+-------------+---------------+-------------------------------------+
|    0        |   1           |   2   				    |
+=============+===============+=====================================+
| energy [eV] | photon number | photon number :math:`\times` energy |
+-------------+---------------+-------------------------------------+

.. code-block:: fortran

   8333.3333333333321        238.86944345907790        6.2633513616217478
   25000.000000000000        182.50447244093024        7.5507336932200397
   41666.666666666664        104.14244180601469        9.1380529004804760
   58333.333333333328        89.422617071344263        9.8579995567498120
   75000.000000000000        70.619159337234422        10.697476969356655
   91666.666666666657        63.363841302196569        11.199199843048401
   ...

The file ``phtnTe001.dat`` records the radiation angular distribution. For example:

+------------------------+------------------------+-----------------------+
|    0                   |   1                    |   2         	  |
+========================+========================+=======================+
| :math:`\theta_z` [rad] | :math:`\theta_y` [rad] | radiated energy [a.u] |
+------------------------+------------------------+-----------------------+

.. code-block:: fortran

   -0.3139E+01    -0.3139E+01     0.0000E+00
   -0.3132E+01    -0.3139E+01     0.0000E+00
   -0.3126E+01    -0.3139E+01     0.0000E+00
   -0.3120E+01    -0.3139E+01     0.0000E+00

For an electron bunch, there are more than 7 outputs, depending on the number of MPI processes. Each output record a sample electron information. On the other hand, file such as ``AveEne(jobno).dat``, ``dist_fn(kstep)(jobno).dat``, ``dist_fn2d(kstep)(jobno).dat`` will be output.

The file ``AveENE`` records:

+----------+-----------------------------+----------------------------------+---------------------------------+
|    0     |   1                         |   2                              | 3                               |
+==========+=============================+==================================+=================================+
| time [s] | average kinetic energy [eV] |  average + :math:`\sigma_+` [eV] | average - :math:`\sigma_-` [eV] |
+----------+-----------------------------+----------------------------------+---------------------------------+

The meaning of :math:`\sigma_\pm` is slightly different from standard deviation. It is defined as:

.. math::

   \sigma_\pm = \frac{\sum |\gamma_i - \langle \gamma \rangle|_\pm}{N_\pm}

where :math:`\sigma_+` is the energy deviation above the average while :math:`\sigma_-` is the energy deviation below the average. :math:`N_\pm` is the number of particles above and below the average energy. In most circumstances, :math:`\sigma_+=\sigma_-` and we can say that the particle energy has a normal distribution. In some cases, a deviation from normal distribution after the interaction may be observed.

The file ``dist_fn`` records:

+-------------+-----------------------+
|    0        |   1                   |
+=============+=======================+
| energy [eV] | electron number [a.u] |
+-------------+-----------------------+

The file ``dist_fn2d`` records:

+--------------------------+--------------------------+------------------------+
|    0                     |   1                      |   2       	       |
+==========================+==========================+========================+
| :math:`p_y` [normalized] | :math:`p_z` [normalized] |  electron number [a.u] |
+--------------------------+--------------------------+------------------------+

.. _python:

Python
------

In this examples, the visualisation is performed by using Python in `Jupyter notebook <https://jupyter.org>`_. The python codes can be found in ``/examples/**.ipynb``. The extension ``.ipynb`` stand for Jupiter notebook. The data can be read as follows:

.. code-block:: python

   #Time evolution of electron energy
   T1,x1,y1,px1,py1,E1,Xi1 = np.loadtxt(rf"{run_dir}/examples/Data1/orbt1q"
                                              +str(file1).zfill(3)+".dat",unpack=True,
                                              usecols=[0,1,2,3,4,5,6],dtype=np.float)
   T2,x2,y2,px2,py2,E2,Xi2 = np.loadtxt(rf"{run_dir}/examples/Data2/orbt1q"
                                              +str(file2).zfill(3)+".dat",unpack=True,
                                              usecols=[0,1,2,3,4,5,6],dtype=np.float)

In the Jupyter notebook, there is a python function ``import figformat``. This function output/display figures with selected parameters. The figure width, **fig_width** is set to 3.4 inches, represents a single column width of a double column journal.

.. code-block:: python

   import matplotlib as mpl
   import figformat
   fig_width,fig_height,params=figformat.figure_format(fig_width=3.4,fig_height=2)
   mpl.rcParams.update(params)

The figure width can be override to any number by writing ``fig.set_size_inches(fig_width*2,fig_width/1.618)`` at each plot. The number ``1.618`` is the Golden ratio. Multiplying or dividing the **fig_width** by the Golden ratio for figure height ensure the nice appearance of a figure. Other parameters such as font size, plot line width, ticks width and etc. can be changed in the file ``figformat.py``.

.. _gnu:

Gnuplot
-------

On the other hand, a quick visualisation can be performed by using `gnuplot <http://www.gnuplot.info>`_. For example:

::

   > plot “***.dat” using ($1):($4) with lines
   > replot “***.dat” using ($1):($4) with lines

.. _examples:

Examples
========

Single electron
---------------

In this example, we plot several outputs of a single electron. Details of the plotting code can be referred to the Jupyter notebook. It can be viewed in GitHub. We showed the output for Lorentz (without RR) and Sokolov (with RR) in classical regime.

The electron trajectory

.. figure:: /figures/trajectories.png

The time evolution of electron energy

.. figure:: /figures/energies.png

The radiation spectrum

.. figure:: /figures/spectra.png

The photon number distribution

.. figure:: /figures/photonnumber.png

Electron bunch
--------------

In this examples, we show the results of :math:`10^9` electrons colliding with the laser with intensity :math:`10^{22}~\mathrm{W cm^{-2}}`. The input is:

.. code-block:: fortran

   &PARAM1  jobno=003,ksmax=1000000,div=60.d0,div2=40.d0                      ,&END
   &PARAM2  SL=1.d22,Ev=600.d6,pw=0.82d-6,pp=3.3d-6,sp=5.5d-6                 ,&END
   &PARAM3  alpha=1.d-6,enum=1.0d9,bin=1.d6,shot=1,inc_ang = 0.d0             ,&END
   &PARAM4  xinit=2.d0,rmass=1.d0,sigmax=0.1d0,sigmay=0.01d0,sigmaz=0.01d0    ,&END
   &PARAM5  iconR=1,QED=0,ipl=0,shape=0,OutRad=1,OutPairs=0                   ,&END
   &PARAM6  loadpar=0,loadseed=33587,qedseed=0                                ,&END

The longitudinal momentum spread is :math:`10\%` of its initial kinetic energy, i.e. ``sigmax=0.1d0``. Other components are set to a very small value. The simulations were run for Sokolov (classical, ``iconR=1, QED=0``), Sokolov (QED-assisted, ``iconR=1, QED=1``), and Stochastic (``iconR=3, QED=1``). For Stochastic, ``QED=1`` is mandatory.

.. figure:: /figures/energies_beam.png

.. figure:: /figures/dist_function.png

.. figure:: /figures/photonnumber_beam.png

Load particle from file
-----------------------

The particle energy distribution can be loaded from the output of PIC simulation. The distribution function file name is ``load_particle.dat``. The data in the first column is the energy in the unit of electron volt [eV]. The second column is the particle distribution. The value of the distribution can be scaled accordingly.

.. figure:: /figures/load_beam.png

Models
======

.. todo:: To do

   Details numerical implementation can be found in Ref. :cite:`mypop`.

Landau-Liftshitz
----------------

.. math::

   \frac{ dv^{\mu}}{d\tau}=\frac{e}{mc}F^{\mu\nu}v_{\nu}+\tau_{0}\left( \frac{e}{mc} \dot{F}^{\mu\nu} v_{\nu}+\frac{e^{2}}{m^{2}c^{2}}F^{\mu\nu}F_{\alpha\nu}v^{\alpha}
   \frac{e^{2}}{m^{2}c^{2}}(F^{\alpha\nu}v_{\nu})(F_{\alpha\lambda}v^{\lambda})v^{\mu}\right)

Sokolov
-------

.. math::

   \frac{ dp^{\mu}}{d\tau}=\frac{e}{mc}F^{\mu\nu}v_{\nu}-\frac{I_{QED}}{mc^2}p^{\mu}+\tau_{0}\frac{e^{2}}{(mc)^{2}}\frac{I_{QED}}{I_{E}}F^{\mu\nu}F_{\nu\alpha}p^{\alpha}

Stochastic
----------



Quantum
-------



Emission cross-section
----------------------

.. math::

   dW_{em}=\frac{\alpha mc^{2}}{\sqrt{3}\pi\hbar\gamma}\left[\left(1-\xi+\frac{1}{1-\xi} \right)K_{2/3}(\delta)
   -\int_{\delta}^{\infty}K_{1/3}(s)ds  \right] d\xi

.. math::

   \xi=\frac{\hbar\omega}{\gamma mc^{2}},\:\delta=\frac{2\xi}{3(1-\xi)\chi}

and :math:`K_{\nu}(x)` is modified Bessel function. At classical limit :math:`\chi<<1`

.. math::

   dP&=&\mathcal{E}dW_{em}\nonumber\\ &\rightarrow& \frac{e^{2}\omega_{c}}{ \sqrt{3}\pi c}\frac{1}{\gamma^{2}}
   \frac{\omega}{\omega}_{c}[2K_{2/3}(\delta)-\int_{\delta}^{\infty}K_{1/3}(s)ds]d\omega

reduced to classical synchrotron radiation where :math:`\omega_{c}` is the critical frequency and :math:`\delta\longrightarrow 2\xi/3\chi`.

.. figure:: /figures/qchi.png

The function :math:`q(\chi_e)~\text{for}~\chi_e\ll 1` (blue)

.. math::

    q(\chi_e\ll 1)\approx 1-\frac{55}{16}\sqrt{3}\chi + 48\chi^2

The function :math:`q(\chi_e)~\text{for}~\chi_e\gg 1` (green)

.. math::

    q(\chi_e\gg 1)\approx\frac{48}{243}\Gamma(\frac{2}{3})\chi^{-4/3}
    \left[ 1 -\frac{81}{16\Gamma(2/3)}(3\chi)^{-2/3} \right]
