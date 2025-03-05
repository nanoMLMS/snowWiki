# About

pySNOW, Software Nano Object Workflow, is written in the [Python :fontawesome-brands-python:](https://www.python.org/) programming language with the aim of providing a user frinedly integrated set of tools for the analysis of nanomaterials from atomistic simulations, it has been built by our team at the University of Milan with some design goals in mind:

## Ease of use

pySNOW functiuons are mostly self explanaotry and relatively easy to use if the user knows a bit about what each descriptor implies, take for instance the function to compute the Steinhardt parameters:

```py title="steinhardt.py"
stein = peratom_steinhardt(index_frame = 1, coords = coords, l = [4, 6], cut_off = 3.52)
```
this function will produce a 2 (number of elements of l)x $n_{atoms}$ array with for each atom (with coordinates in the *coords* array)the Steinhardt parameters of order 4 and 6, considering atoms as neighbours if they are within 3.52 Angstrom of each other.

## Independence

While re-inventing the wheel goes against some of Python's founding principles, we wanted to keep pySNOW as independent as possible from external modules which might break down compatibility down the line. We still kept the widely used [NumPy](https://numpy.org/) and [SciPy](/https://scipy.org/) libraries, without which things would have been quite the nightmare to implement in a clean efficient way, but part from these libraries and [PyTest](https://pytest.org/) for automated testing, pySNOW can be run without any other dependency installed. 

## Integrability

pySNOW contains some input functions for reading some type of files like XYZs and LAMMPS data files, however all functions in pySNOW only require as parameters atomic coordinates and  lists of elements, so that it is possible for users to use any type of functionality to read a structure and use the resulting coordinates as inputs.

For instance one might want to use [ASE](https://wiki.fysik.dtu.dk/ase/) rather than pySNOW to read an XYZ file, so that, to compute the Steinhardt parameters of order 4 and 6 as above:

```py title="using_ase.py"
from ase.io import read
from snow.descriptors.steinhardt import peratom_steinhardt

nano = read("Au_561Ih.xyz")

el = nano.get_chemical_symbols()
coords = nano.get_positions()

stein = peratom_steinhardt(index_frame = 1, coords = coords, l = [4, 6], cut_off = 3.52)
```

rather than use the *read_xyz* function provided with pySNOW:
```py title="only_snow.py"
from snow.lodispp.pp_io import read_xyz
from snow.descriptors.steinhardt import peratom_steinhardt

el, coords = read_xyz("Au_561Ih.xyz")

stein = peratom_steinhardt(index_frame = 1, coords = coords, l = [4, 6], cut_off = 3.52)
```

The advantage of using ASE (or other libraries) is that it supports multiple data files.