(Latest update :: 14:12 2021-12-09)
# Welcome to plantFEM

You can download the [plantFEM](https://github.com/kazulagi/plantfem.git) to perform physical simulation for plants.

[Japanese](index_ja.md)

# Get started!

## (1) Let's download plantFEM into your local system:

```shellscript

git clone https://github.com/kazulagi/plantfem.git && cd plantfem && python3 install.py

```

## (2) or you can run on Google Colab

```
!git clone https://github.com/kazulagi/plantfem.git 
%cd plantfem
!python3 install.py
```

## (3) Also, docker is available


```
docker pull kazulagi/plantfem
docker run --rm -it kazulagi/plantfem /bin/bash
```

Then, let's install plantFEM.


```shellscript 

python3 install.py

```




When you install plantFEM, you'll also get build and execution tools for Fortran since plantFEM is based on the modern Fortran. The plantFEM does lots of things:


- search sample script ``plantfem search``
- create your new project with ``plantfem new``
- build & run your project with ``plantfem run``
- update your plantFEM ``plantfem update``
- check mannual with  ``plantfem man``
- set hostfile for MPI with  ``plantfem hostfile``
- change cpu-core for MPI with  ``plantfem cpu-core``


You can write a script on ``server.f90`` in your root directory of plantFEM. If you want to use other modules written in Fortran, please set them into ``addon/`` directory. All Fortran files in the directory is compiled with the plantFEM library before they are linked to your ``server.f90`` script.


Further, you can easily use MPI by following steps.

- set your hostfile at ``etc/hostfile``
- write number of cpu-cores in ``etc/cpucore`` or run ``plantfem cpu-core`` before running your script.

## How to run your ```.f90``` script?

Single process (Non-MPI):

```
plantfem build && ./server.out
```

With MPI:

```
plantfem build && mpirun -np $NUM_CORE ./server.out
```



## Examples

Sample codes are available in ```Tutorial``` directory.


### Scripts and coding
plantFEM consists of 4 components, each of which contains some libraries for numerical simulations. All examples runs when it is copied and pasted into ``server.f90`` and executed by ``plantfem run`` command.

### std

A standard library of Fortran prepared for plantFEM. This module extends fortran for easier use. 

Try it from HERE >> [std](Tutorial_std.md)

### fem

A library for data-objects for FEM analysis.

Try it from HERE >> [fem](Tutorial_fem.md)

### sim


A library for simulators for FEM analysis.

Try it from HERE >> [sim](Tutorial_sim.md)

### obj

A library for realistic objects such as soybean, stem, leaf, soil, air, light ...etc.

Try it from HERE >> [obj](Tutorial_obj.md)



<!--

```markdown

 Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/kazulagi/plantfem.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
-->