=========================
Spatial Semantic Pointers
=========================


.. image:: https://img.shields.io/pypi/v/nengo_ssp.svg
        :target: https://pypi.python.org/pypi/nengo_ssp

.. image:: https://img.shields.io/travis/nsdumont/nengo_ssp.svg
        :target: https://travis-ci.com/nsdumont/nengo_ssp

.. image:: https://readthedocs.org/projects/nengo-ssp/badge/?version=latest
        :target: https://nengo-ssp.readthedocs.io/en/latest/?badge=latest
        :alt: Documentation Status




Spatial Semantic Pointers (SSPs) for nengo and nengo_spa. Grid cell SSPs included.


* Free software: MIT license
* Documentation: https://nengo-ssp.readthedocs.io.

Installation
--------

To install via pip use::
pip install nengo-ssp


Features
--------

(Note: this package is a work in progress and may change significantly)

A Spatial Semantic Pointer (SSP) class is defined. It is bascially the same as SPs from nengo_sp except fractional binding is defined. To create an SSP,
::
    import nengo_ssp as ssp
    S = ssp.SpatialSemanticPointer(data);

To fractional bind with SSPs,
:: 
    S**3.6

Binding two SSPs together results i an SSP object. Binding an SSP with an SP results in a SP.

To generate hexagonal axis vectors,
::
    X, Y, K = ssp.HexagonalBasis(n_rotates=8,n_scales=8,scale_min=0.8, scale_max=3);

where X and Y are the axis vectors and K is a matrix of the phases of their Fourier transform (useful to have sometimes). One can also obtain ractangular axis vectors with the ssp.vector_generation.RectangularBasis function. A gernarlization of both of these methods is ssp.PlaneWaveBasis, which allows you set K and return the resulting axis vectors (you can set axis vectors to get specfic patterns this way).

A single unitary vector (e.g. for an axis vector) can be obtained via
::
    X = ssp.vector_generation.UnitaryVectors(d)

When using hexagonal axis vectors, one can set encoders of a neural population such that its neurons will have hexagonally tiled firing patterns, like grid cells. To obtian such encoders,
::
    G_encoders, G_sorts = ssp.GridCellEncoders(n_neurons,X,Y)

where G_encoders is the matrix of encoders (G_sorts gives you information on which neurons correpsond to which grid scale/orientation and can be useful for plotting).

Plotting functions are available in ssp.plotting. A methof for plotting SSP heat maps/similarity plots is
::
    sim_dots, S_list = ssp.plotting.similarity_plot( X, Y, xs, ys, x0=0,y0=0,S0=None,S_list=None);

This will plot the heat map: the dot product of an SSP S0 (which can be provided or its x,y, coordinates can be given in x0,y0) with SSPs tiling a space. The x coordinates of the tiling are given in xs and the y coordinates in ys. X, Y are the axis vectors. This returns a matrix of the similarity values (sim_dots) and a list of all the tiled SSPs (S_list) -- if the same tiled space will be used for several heat maps then this can be reused to speed things up.

A path integration network is given in ssp.networks.PathIntegrator.

Utilities are in ssp.utils. These are a bunch of random fuctions that I've found useful. These include

- ssp.utils.ssp_vectorized(basis, positions):  Given a matrix of axis vectors, d by n (d = dimension of semantic pointer basis vectors, n = number of axis  vectors), and a matrix of positions, N by n (N = number of points) return a matrix of N ssp vectors representing those points.

- ssp.utils.random_path(radius, n_steps, dims, fac): Return a random walk with n_steps # of steps in dims dimensions, bounded in a square [-radius,radius]x[-radius,radius]. fac determines how 'fast' the walk is.

- ssp.utils.circle_rw(n,r,x0,y0,sigma) : Return a random walk with n # of steps in 2 dimensions, bounded in a circle of radius r, and centred at x0,y0. sigma determines how 'fast' the walk is.

- ssp.utils.generate_signal(T,dt,dims = 1, rms=0.5,limit=10, seed=1): Return a smooth radnom path ( a freq limited signal) in dims dimensions, that lasts T time with timesteps dt.


Credits
-------

This package was created with Cookiecutter_ and the `audreyr/cookiecutter-pypackage`_ project template.

.. _Cookiecutter: https://github.com/audreyr/cookiecutter
.. _`audreyr/cookiecutter-pypackage`: https://github.com/audreyr/cookiecutter-pypackage
