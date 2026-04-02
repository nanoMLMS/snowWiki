# Common Neighbour Analysis

The common neighbour analysis is a signature attributed to all pairs of nearest neighbour atoms in the system used to classify the geometrical environment of the atoms. For each pair of neighbours we evaluate a triplet of integers: (r, s, t) as:

=== "r"

    Number of shared neighbours of the pair, here the two green atoms share 4 neighbours (coloured in blue) so in this example the pair will have **r = 4**
    ![Image title](assets/cna_r.png){ align=left }

=== "s"

    Number of bonds between the r nieghbours (excluding pair), here you can see that there are two bonds between the 4 neighbours of the pair taken into consideration, so **s = 1**
    ![Image title](assets/cna_s.png){ align=left }

=== "t"

    Longest path along connected bonds between the r neighbours (excluding pair), here you can see that the two neighbours only form a path of length 1, so **t = 1**
    ![Image title](assets/cna_t.png){ align=left }


## Computing the pair CNA Signature

As said, the CNA signature is a pair property, in pySNOW we can compute the CNA for each pair of atom and obtain it as an $3\cdot N_{pairs}$ array where for each pair the subarray contains the (r, s, t) values, optionally the function will also return a list containing the indeces of the atoms forming each pair for which the CNA signature has been computed, this can be used (and is used by other fnctions which we will present below) to identify patterns in the observed CNA signatures.

The method to compute the CNA involves, as always, as a first step, the reading of the coordinates of the atoms, then the function `#!python calculate_cna(...)` can be used to obtain the desired output:

=== "Using pySNOW io"

    ```py linenums="1"
    from snow.io import read_xyz
    from snow.descriptor.cna import calculate_cna

    el, coords = read_xyz("Au976To.xyz")
    cut_off = 4.079 * 0.85 # (1)
    n_pairs, cnas, pairs = calculate_cna(1, coords, cut_off, return_pair = True) # (2)
    ```

    1. Cutoff as 0.85*lattice parameter
    2. The first 1 is the index_frame placeholder. Setting *return_pair* to **True** will return the indeces of the atoms forming each pair

=== "Using ASE io"

    ```py linenums="1"
    from ase.io import read
    from snow.descriptors.cna import calculate_cna

    nano = read("Au976To.xyz")

    coords = nano.get_positions()

    cut_off = 4.079 * 0.85 # (1)
    n_pairs, cnas, pairs = calculate_cna(1, coords, cut_off, return_pair = True) # (2)
    ```

    1. Cutoff as 0.85*lattice parameter
    2. The first 1 is the index_frame placeholder. Setting *return_pair* to **True** will return the indeces of the atoms forming each pair

=== "Using Ovito io"

    ```py linenums="1"
    from from ovito.io import import_file

    from snow.descriptors.cna import calculate_cna

    pipeline = import_file("Au976To.xyz")

    coords = nano.get_positions()

    cut_off = 4.079 * 0.85 # (1)
    n_pairs, cnas, pairs = calculate_cna(1, coords, cut_off, return_pair = True) # (2)
    ```

    1. Cutoff as 0.85*lattice parameter
    2. The first 1 is the index_frame placeholder. Setting *return_pair* to **True** will return the indeces of the atoms forming each pair


## Computing patterns

## Computing patterns

An easier structural descriptor to analyze is provided by **CNA patterns (CNAps)**, which are **single-particle properties** obtained by enumerating the CNA signatures in which a given particle participates.

For example, a particle with 12 nearest neighbors participates in 12 CNA signatures. The *type* of these signatures provides direct information about the local symmetry and structural environment. In an ideal FCC crystal, all 12 signatures are of type `(421)`, whereas an icosahedral center is characterized by 12 `(555)` signatures.

We denote a CNA pattern as a list whose elements consist of the number of occurrences followed by the corresponding signature. In the simple cases above, these would be written as:

- `(12, (421))` for an FCC atom  
- `(12, (555))` for an icosahedral center  

More complex local environments give rise to mixed patterns. For instance, an atom lying on a five-fold symmetry axis is identified by the pattern:
- `[(10, (422)), (2, (555))]`
indicating participation in ten `(422)` signatures and two `(555)` signatures.


CNAPs are described in the following table, also available in the readme of the library:

#### **CNAp Index Descriptions**  

| **CNAp** | **Description**                                      | **CNAp composition** |
|:--------:|------------------------------------------------------|:--------:|
| 1        | Vertex between two (111) facets and a (100) facet    |[(1, (100)), (2, (211)), (1, (322)), (1, (422))] |
| 2        | Edge between (100) and a slightly distorted (111)    |[(1, (200)), (2, (211)), (2, (311)), (1, (421))] |
| 3        | Atoms lying on a (555) symmetry axis                 |[(10, (422)), (2, (555))] |
| 4        | FCC bulk                                             |[(12, (421))] |
| 5        | Intersection of six five-fold axes                   |[(12, (555))] |
| 6        | Edge between (100) facets                            |[(2, (100)), (2, (211)), (2, (422))] |
| 7        | Vertex on twinning planes shared by (111) facets     |[(2, (200)), (1, (300)), (2, (311)), (1, (322)), (1, (422))] |
| 8        | Edge between (111) re-entrances and (111) facets     |[(2, (200)), (4, (311)), (1, (421))] |
| 9        | Re-entrance delimited by (111) facets                |[(2, (300)), (4, (311)), (2, (421)), (2, (422))] |
| 10       | Edge between (100) and (111) facets                  |[(3, (211)), (2, (311)), (2, (421))] |
| 11       | Vertex shared by (100) and (111) facets              |[(4, (211)), (1, (421))] |
| 12       | (100) facet                                          |[(4, (211)), (4, (421))] |
| 13       | Five-fold symmetry axis (without center)             |[(4, (311)), (2, (322)), (2, (422))] |
| 14       | Five-fold vertex                                     |[(5, (322)), (1, (555))] |
| 15       | (111) face                                           |[(6, (311)), (3, (421))] |
| 16       | Twinning plane                                       |[(6, (421)), (6, (422))] |
| 19       | Reentrance in (100) facet                            |[(4, (311)), (7, (421))] |


To compute the patterns there are two functions implemented in pysnow, one computes the patterns explicitly, producing a list of tuples (signatures, count) for all atoms, useful for an overall statistic of the patterns, the other computes the pattern nternally and then returns a list of integers as those in table above for a more strightforward identification of known structures, also useful for visualization purposes.

Both functions only require the coordinates of the atoms and a cutoff distance for the identification of nearest neighbours.

=== "Using pySNOW io"

    ```py linenums="1"
    from snow.io import read_xyz, write_xyz
    from snow.descriptors.cna import cnap_peratom

    el, coords = read_xyz("Au976To.xyz")
    cut_off = 4.079 * 0.85 
    cnap_indeces = cnap_peratom(1, coords, cut_off, display_progress = True) # (1)
    write_xyz("Au976To_cnap.xyz", el, coords, additional_data = cnap_indeces.reshape(-1,1)) # (2)
    ```

    1. If tqdm is installed you can opt to display a progress bar with display_progress = True
    2. A bit annoying but the write_xyz of snow is a bit dumb, it requires some reshaping

After computation of the CNAPs for all atoms the resulting structure, saved to xyz with an additionl column for the CNAPs can be visualized in Ovito

![Image title](assets/au976cnap.png){ align=center }


Ovito users can also make use of our Ovito modifier [CNAPatterner](https://github.com/nanoMLMS/CNAPatternerOvito/) which provides a similar kind of analysis. While Ovito pipelines are generally tricky to deal with in python, using it is advisable when treating with larger systems and/or systems with periodic boundary conditions.
