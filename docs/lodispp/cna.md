# Common Neighbour Analysis

pySNOW contains functions that allow users to compute CNA signature for each pair of nearest neighbours as well as identify to which signature
each atom in the system partecipates to. The latter is especially useful in cathegorizing the atom based on its belonging to a particular geometrical
structure, extending what done in Ovito, where atoms are only catheorized based on the local cristalline structure, allowing to identify atoms belonging 
to a facet, an edge or a vertex.

The CNA signature is a triplet of numbers (r, s, t) for each pair of neighbours where:

- r: number of common neighbours of the pair
- s: number of bonds between the r common neighbours
- t: the longest path that can be formed by joining the r neighbours that are neighbours of each other



## Functions

The CNA calculator containes a series of functions for the comutation of the CAN signatures and patterns:

### Calculate CNA Fast



::: snow.lodispp.cna.calculate_cna_fast

### Write CNA to file

::: snow.lodispp.cna.write_cna

### CNA Patterns per atoms

::: snow.lodispp.cna.cna_peratom

### CNAp Index per atom

Usage:
```python
cnap_peratom(index_frame: int, coords: np.ndarray, cut_off: float = None, pbc: bool = False) -> np.ndarray
```

::: snow.lodispp.cna.cnap_peratom