Table of Contents
=================

* [GEOSmie](#geosmie)
   * [Mie code](#mie-code)
   * [Main code](#main-code)
      * [Calculations at individual wavelengths](#calculations-at-individual-wavelengths)
      * [Calculations over wavelength bands](#calculations-over-wavelength-bands)
   * [GSF expansion code](#gsf-expansion-code)

<!-- Created by https://github.com/ekalinin/github-markdown-toc -->


# GEOSmie

See [here (TBD)](tbd) for a full documentation. Below is a brief summary of the structure and usage of the package.

The package consists of the following parts:

- Mie code (pymiecoated/)
- Main code (root directory), which can calculate single-scattering and bulk optical properties for both individual wavelengths and wavelength bands
- Generalized spherical function expansion code (gsf/)

## Mie code

Before any spherical aerosol calculations can be done the Mie code needs to be installed.

Starting in the root directory of this repository:

```bash
cd pymiecoated
python setup.py install
```

In some systems the following may be needed instead:

```bash
python setup.py install --user
```

The Mie code does not need to be used directly; runoptics.py calls it as needed. The functions within the module can be used directly to run custom Mie simulations but that is beyond the scope of this README.


## Main code

The main single-scattering property calculator consists of two parts: calculation at individual wavelengths, and calculation over wavelength bands. The first part is always necessary, the second needs to be run only if wavelength band integrated files are necessary.

### Calculations at individual wavelengths

To calculate single-scattering properties at individual wavelengths:

```bash
python runoptics.py --name path/to/file.json
```

Where file.json defines all of the parameters of the calculation. ".json" can be omitted from the runoptics.py command as a convenience. See JSON examples under geosparticles/ for current GEOS aerosol particles. For users who only want single-scattering properties at individual wavelengths this is all that is needed, and further processing is not needed.

runoptics.py has a few additional options, use

```bash
python runoptics.py --help
```

to see them

### Calculations over wavelength bands

To calculate single-scattering properties at wavelength bands:

```bash
python runbands.py --filename filename.nc
```

where filename.nc should be an output file from runoptics.py

For additional options, see

```bash
python runbands.py --help
```

## GSF expansion code

The generalized spherical function expansion code is needed to convert the phase functions in the optical tables to be compatible with the VLIDORT radiative transfer software. It is not needed unless you are running VLIDORT directly.

The code uses spher_expan.f code by Michael Mishchenko. Before conversions can be done the Fortran code needs to be compiled. 

Starting in the root directory of this repository:

```bash
cd gsf
gfortran spher_expan.f
```

Other compilers beyond gfortran may work. Testing has been performed with gfortran 11.2.0 on Gentoo.

To run the conversion code, assuming the code has been compiled and you are in the gsf/ subdirectory:

```bash
python rungsf.py --filename ../filename.nc
```

where ../filename.nc should be an output file from runoptics.py

For additional options, see

```bash
python rungsf.py --help
```
