[plantFEM](https://kazulagi.github.io/plantfem.github.io/index.md) >> [std](https://github.com/kazulagi/plantfem/tree/master/src/std/std.f90)

## plantFEM std library

Welcome to a tutorial for plantFEM std library.

The std library contains following classes and modules ([std](https://github.com/kazulagi/plantfem/tree/master/src/std/std.f90)).

### modules

Followings are major classes.

#### (1) plantFEM uses [iso_fortran_env](https://gcc.gnu.org/onlinedocs/gfortran/ISO_005fFORTRAN_005fENV.html) for basic data types. 

For instance, 64-bit Real value and 32-bit Integer value are defined as shown below.


```fortran

program main
    use iso_fortran_env
    implicit none


    ! defining 128-bit Real value 
    real(real128) :: re128 = 1.0d0

    ! defining 64-bit Real value 
    real(real64) :: re64 = 1.0d0

    ! defining 32-bit Real value 
    real(real32) :: re32 = 1.0d0


    ! defining 64-bit Integer value 
    integer(int64) :: in64 = 1234567890

    ! defining 32-bit Integer value 
    integer(int32) :: in32 = 1234567890

    ! defining 16-bit Integer value 
    integer(int16) :: in16 = 12345

    ! defining 8-bit Integer value 
    integer(int8) :: in8 = 123

    print *, re128
    print *, re64
    print *, re32
    print *, in64
    print *, in32
    print *, in16
    print *, in8

end program main

```

If the script is named as "server.f90" and located in the root repository of the plantfem, the script is executable by this command.

```shellscript

./plantfem run

```


The result of the script is,

```shellscript

   1.00000000000000000000000000000000000      
   1.0000000000000000     
   1.00000000    
           1234567890
  1234567890
  12345
  123

```

(2) [TimeClass](https://github.com/kazulagi/plantfem/tree/master/src/TimeClass/TimeClass.f90) is a class for time measurement, which can be used to measure cpu time by creating an instance of type time_.

(3) [MathClass](https://github.com/kazulagi/plantfem/tree/master/src/MathClass/MathClass.f90) is a library for mathematical operations such as matrix calculation and tensor operations. Although "Class" is in the name, but this is just a toolbox of useful functions.

(4) [HTTPClass](https://github.com/kazulagi/plantfem/tree/master/src/HTTPClass/HTTPClass.f90) is a remnant from when I was trying to set up an http server at plantFEM. It is waiting for your commit.

(5) [IOClass](https://github.com/kazulagi/plantfem/tree/master/src/IOClass/IOClass.f90) is a class to realize a file operation like Python in Fortran. You can create an instance of type "IO_" for easy file operation.

(6) [RandomClass](https://github.com/kazulagi/plantfem/tree/master/src/RandomClass/RandomClass.f90) is a class in Fortran to do the same thing as the random class in Python that does pseudo-random number generation.

(7) [ArrayClass](https://github.com/kazulagi/plantfem/tree/master/src/ArrayClass/ArrayClass.f90) is a toolbox of functions and subroutines that perform array operations, similar to Python's numpy.

(8) [GraphClass](https://github.com/kazulagi/plantfem/tree/master/src/GraphClass/GraphClass.f90) is a class for implementing the graph structure and performing operations. Implementation in progress.

(9) [MPIClass](https://github.com/kazulagi/plantfem/tree/master/src/MPIClass/MPIClass.f90) is class as a wrapper of MPI in terms of Fortran. You can easily perform MPI parallelization by using its instance.

(10) In [DictionaryClass](https://github.com/kazulagi/plantfem/tree/master/src/DictionaryClass/DictionaryClass.f90), we implemented the Python equivalent of a dictionary class. To be honest, I don't use it much.

(11) [OpenMPClass](https://github.com/kazulagi/plantfem/tree/master/src/OpenMPClass/OpenMPClass.f90) is class as a wrapper of OpenMP in terms of Fortran. [MPIClass](https://github.com/kazulagi/plantfem/tree/master/src/MPIClass/MPIClass.f90) is very good and useful, but this class is honestly a lost cause and needs a vision.

(12) [LinearSolverClass](https://github.com/kazulagi/plantfem/tree/master/src/LinearSolverClass/LinearSolverClass.f90) is a solver for linear equations. It is an original implementation. It is planned to be replaced in the future by the well-known MKL and LAPACK.

(13) [TreeClass](https://github.com/kazulagi/plantfem/tree/master/src/TreeClass/TreeClass.f90) is a class for tree graphs. The maintenance is currently suspended. This class is looking for a vision.

(14) [ShapeFunctionClass](https://github.com/kazulagi/plantfem/tree/master/src/ShapeFunctionClass/ShapeFunctionClass.f90) is a class for the shape function. Currently we only use 2D 4-node iso-parametric elements and 3D 8-node iso-parametric elements. We are looking for committers to implement other types of elements.


... and others are miner.
[TermClass](https://github.com/kazulagi/plantfem/tree/master/src/TermClass/TermClass.f90), 
[PhysicsClass](https://github.com/kazulagi/plantfem/tree/master/src/PhysicsClass/PhysicsClass.f90),
[KinematicClass](https://github.com/kazulagi/plantfem/tree/master/src/KinematicClass/KinematicClass.f90),
[VertexClass](https://github.com/kazulagi/plantfem/tree/master/src/VertexClass/VertexClass.f90), 
[CSVClass](https://github.com/kazulagi/plantfem/tree/master/src/CSVClass/CSVClass.f90),
[VectorClass](https://github.com/kazulagi/plantfem/tree/master/src/VectorClass/VectorClass.f90),
[EquationClass](https://github.com/kazulagi/plantfem/tree/master/src/EquationClass/EquationClass.f90),
[GeometryClass](https://github.com/kazulagi/plantfem/tree/master/src/GeometryClass/GeometryClass.f90),
[RouteOptimization](https://github.com/kazulagi/plantfem/tree/master/src/RouteOptimization/RouteOptimization.f90),
[STLClass](https://github.com/kazulagi/plantfem/tree/master/src/STLClass/STLClass.f90) and
[WebserverClass](https://github.com/kazulagi/plantfem/tree/master/src/WebserverClass/WebserverClass.f90).