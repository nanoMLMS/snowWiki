# pySNOW

pySNOW (Software Nano Object Workflow) is a set of [Python :fontawesome-brands-python:](https://www.python.org/) tools for the structural analysis of atomic configurations originating from MD and other atomistic simulations. The code is freely available on [GitHub](https://github.com/nanoMLMS/pySNOW).


<div class="grid cards" markdown>

- :fontawesome-brands-github: __GitHub__ Get pySnow on [GitHub](https://github.com/nanoMLMS/pySNOW) 

</div>

```python
#example: reading an xyz and computing the aGCN and writing the per-atom aGCN to an xyz
from snow.lodispp.pp_io import read_xyz, write_xyz
from snow.descriptors.gcn import agcn_calculator

el, sys_coords = read_xyz(filename = "Au561_Ih.xyz")
agcn = agcn_calculator(index_frame=1, coords=coords, cutoff=cutoff_radius, gcn_max=12)
agcn.reshape(-1, 1)

write_xyz("output_test.xyz", elements=elements, coords=coords, additional_data=additional_data)
```
