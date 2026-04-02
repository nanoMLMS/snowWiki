# Distribution Functions

Distribution funtions are useful for identifying how atoms are ddistributed spatially, pySNOW allows for the computation of two such descriptors, the PDDF[^1] and the RDF[^2], for both of these descriptors it is also possible to evaluate such descriptors globally for all atoms or a chemical distribution function, which will isolate distribution of atoms of the same chemical species.

## PDDF

The PDDF is computed as a binning of the distances between all pairs of atoms inside the system. Snow allows for the computation of the PPDF choosing either the number of bins or the size of the indivisual bins, the function itself has the following parameters_

- index_frame (integer): useful mostly for analysing trajectories and keeping track of the frame number, if dealing with a snapshot just assign it as 1
- coords (array): the array of coordinates of the atoms for the snapshot/ trajectory frame
- bin_precision (float): size of the individual bin in Angstrom
- bin_count (integer): the number of bins

Note that only one of the final two parameters has to be specified, the other is computed automatically starting from the specified one and the size of the system:

$$
l_{bin} = d_{max} / N_{bins}
$$

specifying both will favour bin_precision as it is the dirst to be considered in an if statement in the code.

### Usage example

In this snippet we will compute the PDDF for a 561 atoms large cluster of gold atoms.

```py title="pddf.py" linenums="1"
from snow.io import read_xyz
from snow.descriptors.distributions import pddf_calculator
import matplotlib.pyplot as plt
# read a structure from an xyz file to a list of elements
# and array of coordinates
el, coords = read_xyz("Au561_Ih.xyz")
```

after reading the coordinates we compute the PDDF, the function has two returns:
- The interatomic distance array with the equivalent distance for each bin
- The number of pairs separated by an interatomic distance within each bin

Here we use a bin_prescision of 0.5 Angstrom and then plot the distribution
```py title="pddf.py linenums="1"
dist, count = pddf_calculator(1, coords, bin_precision = 0.5)

plt.plot(dist, count)

```
## PDDF By Element
The study of nano alloys might require to know how atoms of a specific element contributing to the alloy are distributed, pySnow thus allow users to copmpute the PDDF restricted to a single element fo the alloy using the **pddf_calculator_by_element** function. This function has the same parameters as the general PDDF calculator save for a string field identified as **element**, moreover it requires users to pass the ordered list of elements as read from the coordinate files to the function.

## Example

In this example we shall study a gold-platinum alloy, we compute the partial PDDF for both gold and platinum and for the whole system then we plot it.

```py title="partial_pddf.py" linenums="1"
from snow.io import read_xyz
from snow.descriptors.distributions import pddf_calculator
import matplotlib.pyplot as plt
# read a structure from an xyz file to a list of elements
# and array of coordinates
el, coords = read_xyz("AuPt559.xyz")

dist_au, count_au = pddf_calculator_by_element(1, coords, el, 'Au', 0.5)
dist_pt, count_pt = pddf_calculator_by_element(1, coords, el, 'Pt', 0.5)
dist, count = pddf_calculator(1, coords, bin_precision = 0.5)

plt.plot(dist_au, count_au, label ="PDDF Au")
plt.plot(dist_pt, count_pt, label ="PDDF Pt")
plt.plot(dist, count, label ="PDDF total")
plt.legend()
plt.xaxis("d [Angs]")
plt.yaxis("Occurance")

```

## RDF

The RDF is a distribution


[^1]: Pair Distance Distribution Function

[^2]: Radial Distribution Function