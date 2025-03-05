# Common Neighbour Analysis

The common neighbour analysis is a signature attributed to all pairs of nearest neighbour atoms in the system used to classify the geometrical environment of the atoms. For each pair of neighbours we evaluate a triplet of integers: (r, s, t) as:

=== "r"

    Number of shared neighbours of the pair
    ![Image title](assets/cna_r.png){ align=left }

=== "s"

    Number of bonds between the r nieghbours (excluding pair)
    ![Image title](assets/cna_s.png){ align=left }

=== "t"

    Longest path along connected bonds between the r neighbours (excluding pair)
    ![Image title](assets/cna_s.png){ align=left }