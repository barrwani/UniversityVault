# Reframe



![[Pasted image 20250228020742.png]]


```python
site_configuration = {
    'systems': [
        {
            'name': 'my_hpc_cluster',
            'descr': 'My HPC cluster using Slurm',
            'hostnames': ['mycluster'],
            'modules_system': 'lmod',  # Change this if your system uses another module system
            'partitions': [
                {
                    'name': 'compute',
                    'scheduler': 'slurm',
                    'launcher': 'srun',
                    'environs': ['builtin'],
                }
            ]
        }
    ],
    'environments': [
        {
            'name': 'builtin',
            'cc': 'gcc',
            'cxx': 'g++',
            'ftn': 'gfortran'
        }
    ]
}
```