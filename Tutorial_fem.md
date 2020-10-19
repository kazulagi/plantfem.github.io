[plantFEM](https://kazulagi.github.io/plantfem.github.io) >> [fem](https://kazulagi.github.io/plantfem.github.io/Tutorial_fem.html)


# plantFEM fem library

Welcome to a tutorial for plantFEM fem library.

The fem library contains following classes and modules ([fem](https://github.com/kazulagi/plantfem/tree/master/src/fem/fem.f90)).

## modules

Followings are major classes.


### (1) [MeshClass](https://github.com/kazulagi/plantfem/tree/master/src/MeshClass/MeshClass.f90) is a class for mesh objects, the instance of which can have all information for a plain mesh object.

### (2) [MaterialPropClass](https://github.com/kazulagi/plantfem/tree/master/src/MaterialPropClass/MaterialPropClass.f90) is a class for material properties, the instance of which can have all material information for single plain mesh object.

### (3) [BoundaryConditionClass](https://github.com/kazulagi/plantfem/tree/master/src/BoundaryConditionClass/BoundaryConditionClass.f90) is a class for initial/boundary conditions properties, the instance of which can have all initial/boundary conditions information for single plain mesh object.

### (4) [ControlParaeterClass](https://github.com/kazulagi/plantfem/tree/master/src/MeshClass/MeshClass.f90) is a class for control parameters for FEM analysis, the instance of which contains tolerance and dt.

### (5) [FEMDomainClass](https://github.com/kazulagi/plantfem/tree/master/src/FEMDomainClass/FEMDomainClass.f90) is a class for domain objects for FEM analysis, the instance of which can have all information for a strong-coupling FEM analysis.

### (6) [PreProcessingClass](https://github.com/kazulagi/plantfem/tree/master/src/PreProcessingClass/PreProcessingClass.f90) is a class for pre-processing in terms of FEM. This class is regacy, hence, under refactoring and being integrated into FEMDomainClass.

<!--
```fortran

program main

end program main

```



If the script is named as "server.f90" and located in the root repository of the plantfem, the script is executable by this command.

```shellscript

./plantfem run

```


The result of the script is,

```shellscript


```

These are minor classes.

[StrainClass](https://github.com/kazulagi/plantfem/tree/master/src/StrainClass/StrainClass.f90)
[StressClass](https://github.com/kazulagi/plantfem/tree/master/src/StressClass/StressClass.f90)
[ConstitutiveModelClass](https://github.com/kazulagi/plantfem/tree/master/src/ConstitutiveModelClass/ConstitutiveModelClass.f90)
[FEMIfaceClass](https://github.com/kazulagi/plantfem/tree/master/src/FEMIfaceClass/FEMIfaceClass.f90)
[PostProcessingClass](https://github.com/kazulagi/plantfem/tree/master/src/PostProcessingClass/PostProcessingClass.f90)

-->