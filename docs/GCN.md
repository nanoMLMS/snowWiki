# GCN Computation

The GCN is an extended and generalized (as the name: Generalized Coordination Number implies) version of the Coordination Number (CN), it can be used to capture trends in adsorption nergy on surfaces and on nanoparticles.


While the CN is just a count for each atom i the system of the number of other atoms that lie within a certain cutoff distance of the first atom itself, the GCN (or $\bar{CN}$) is a weighted average of the CN of the atom neighbouring each atom in the system, that is:

$$
\bar{CN}(i) = \sum_{j\in nn}\frac{CN(j)}{CN_{max}}
$$

where $CN_{max}$ is the maximal CN for an atom in the bulk, for instance, in the case of fcc $CN_{max} = 12$.

## Atop GCN

The equation presented above is for the **atop GCN**, that is the GCN for an adsorption site corresponding to the atom itself, to compute such value in pySNOW we first need to load a structure from a coordinate file, here we will use the builtin function to read from an XYZ file, however any library that can read or generate coordinates for an atomic structure can be used to prepare the coordinate array required by the code.

```py title="agcn.py" linenums="1"
from snow.lodispp.pp_io import read_xyz
from snow.descriptors.gcn import agcn_calculator

# read a structure from an xyz file to a list of elements
# and array of coordinates
el, coords = read_xyz("Au561_Ih.xyz")

# impose a cutoff of 0.85 * lattice parameter, 4.079 Angstrom for gold
cut_off = 4.079 * 0.85

# compute the aGCN and save it as a 1d array
agcn = agcn_calculator(1, coords, cut_off, gcn_max = 12.0) # (1)!
```

1.  The first parameter is a placeholder for the frame number, this can be usefule to keep trak of the frame in a trajectory
and potentially be used inlater version to automate anlysis of a trajectory 

Once the agcn for each atom has been computed the array can be saved as the extra column of an extended XYZ for visual representation using a software such as [Ovito](https://www.ovito.org/ "Get the best viz tool!") using the builtin function that allows users to write coordinates and any additional per-atom data to an XYZ file.

```py title="agcn.py" linenums="13"
from snow.lodispp.pp_io import write_xyz

agcn.reshape(-1, 1) # (1)!

write_xyz("Au561_Ih_aGCN.py", el, coords, additional_data=agcn)
```

1. It is necessary to reshape the array into a column to save it to the xyz

## Bridge GCN

While the aGCN can be seen as an atomic descriptor, the GCN itself is, as mentioned in the beginning, a useful descriptor related to adsorption sites, and these can also be located between atoms. The simplest conceptually is the bridge site, located at the midpoint between a pair of atoms.

The computation of the bGCN is similar to that of the aGCN, however here the average is over the CNs of the atoms that are neighbours of both atoms taken once, the weight, $CN_{max}$ is in this case 18 for fcc.

To ease visual representation the function for the computation of the bGCN allows for the optional output of the coordinates of the midpoints between the considered pairs of atoms, 