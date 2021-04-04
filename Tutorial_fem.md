[plantFEM](https://kazulagi.github.io/plantfem.github.io) >> [fem](https://kazulagi.github.io/plantfem.github.io/Tutorial_fem.html)


# plantFEM fem library

Welcome to a tutorial for plantFEM fem library.

The fem library contains following classes and modules ([fem](https://github.com/kazulagi/plantfem/tree/master/src/fem/fem.f90)).

## modules

Followings are major classes.


### (1) [MeshClass](https://github.com/kazulagi/plantfem/tree/master/src/MeshClass/MeshClass.f90) is a class for mesh objects, the instance of which can have all information for a plain mesh object.

Ex. mesh creation, export, and get informations

```fortran

program main
    use fem
    implicit none

    type(Mesh_) :: cube, sphere, cylinder
    real(real64),allocatable :: coordinate(:)

    ! create new plain mesh
    call cube%create(meshtype="Cube",x_num=10,y_num=10,division=10,x_len=1.0d0,y_len=1.0d0,thickness=1.0d0)
    call sphere%create(meshtype="Sphere",x_num=10,y_num=10,division=10,x_len=1.0d0,y_len=1.0d0,thickness=1.0d0)
    call cylinder%create(meshtype="Cylinder",x_num=10,y_num=10,division=10,x_len=1.0d0,y_len=1.0d0,thickness=1.0d0)

    ! export as json files
    call cube%json(name="cube.json",endl=.true.)
    call sphere%json(name="sphere.json",endl=.true.)
    call cylinder%json(name="cylinder.json",endl=.true.)


    ! get number of nodes
    print *, "Number of nodes (cube) : ", cube%numNodes()
    ! get number of elements
    print *, "Number of elements (cube) : ", cube%numElements()

    ! remove elements and nodes in a range
    call cube%remove(x_min=0.20d0,x_max=2.0d0)
    ! get number of nodes
    print *, "Number of nodes (cube) : ", cube%numNodes()
    ! get number of elements
    print *, "Number of elements (cube) : ", cube%numElements()

    ! get coordinate
    ! (x, y, z) of node, the ID of which is 10
    coordinate = cube%getCoordinate(NodeID=11)
    print *, "x, y, z :",coordinate(:)

    ! x-coordinate (x1, x2, x3, ... xn) of all nodes
    coordinate = cube%getCoordinate(onlyY=.true.)
    print *, "(x1, x2, x3, ... xn) :",coordinate(:)

end program main

```


The json file contains such data as


```json

{
 "name": "cube.json",
 "mesh":{
"nodcoord" : [
[-0.00000000,-0.00000000,-0.00000000],
[0.10000000,-0.00000000,-0.00000000],
[0.20000000,-0.00000000,-0.00000000],
[0.30000000,-0.00000000,-0.00000000],
...],
"ElemNod" : [
[1,2,13,12,122,123,134,133],
[2,3,14,13,123,124,135,134],
[3,4,15,14,124,125,136,135],
[4,5,16,15,125,126,137,136],
...],
"ElemMat" : [1,1,1,...]
 },
 "return" : 0
}

```

Hence, we can read and visualzie by using other languages such as python.

```python

import json
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D

json_file = open('cube.json', 'r')
json_object = json.load(json_file)

# メッシュオブジェクト中の節点座標配列を取り出す

nodcoord = np.array(json_object["nodcoord"])

# 以下、matplotlibで描画
#x軸とy軸にラベル付け
fig = plt.figure()
ax = Axes3D(fig)

ax.set_xlabel("x")
ax.set_ylabel("y")
ax.set_zlabel("z")

# 節点を描画

x = nodcoord[:,0]
y = nodcoord[:,1]
z = nodcoord[:,2]
ax.plot(x,y,z,marker="o",linestyle='None')

# 図を表示

plt.show()

```


### (2) [MaterialPropClass](https://github.com/kazulagi/plantfem/tree/master/src/MaterialPropClass/MaterialPropClass.f90) is a class for material properties, the instance of which can have all material information for single plain mesh object.

### (3) [BoundaryConditionClass](https://github.com/kazulagi/plantfem/tree/master/src/BoundaryConditionClass/BoundaryConditionClass.f90) is a class for initial/boundary conditions properties, the instance of which can have all initial/boundary conditions information for single plain mesh object.

### (4) [ControlParaeterClass](https://github.com/kazulagi/plantfem/tree/master/src/MeshClass/MeshClass.f90) is a class for control parameters for FEM analysis, the instance of which contains tolerance and dt.

### (5) [FEMDomainClass](https://github.com/kazulagi/plantfem/tree/master/src/FEMDomainClass/FEMDomainClass.f90) is a class for domain objects for FEM analysis, the instance of which can have all information for a strong-coupling FEM analysis.

Ex. Mesh creation and save

```fortran

program main
    use fem
    implicit none

    type(FEMDomain_) :: obj1
    type(FEMDomain_) :: obj2
    type(FEMDomain_) :: obj3
    type(FEMDomain_) :: obj4
    type(FEMDomain_) :: obj5

    ! create FEM domain entities

    ! -----> 1D
    call obj1%create(meshtype="Bar1D",x_num=10,x_len=10.0d0)

    ! -----> 2D
    call obj2%create(meshtype="rectangular2D",x_num=12,y_num=12,x_len=5.0d0,y_len=50.0d0)
    call obj2%gmsh(Name="obj2")
    
    ! -----> 3D
    call obj3%create(meshtype="Cube",x_num=10,y_num=12,z_num=10,x_len=5.0d0,y_len=50.0d0,z_len=10.0d0)
    
    ! export .pos for Gmsh
    call obj3%gmsh(Name="obj3")
    
    call obj4%create(meshtype="Sphere",x_num=10,y_num=10,z_num=10,x_len=5.0d0,y_len=50.0d0,z_len=10.0d0)
    call obj4%resize(x=1.0d0,y=1.0d0,z=1.0d0)    
    ! export .pos for Gmsh
    call obj4%gmsh(Name="obj4")
    
    ! move 1.0 to x direction
    call obj4%move(x=1.0d0)
    
    ! rotate 30.0 degrees around x-axis
    call obj4%rotate(y=radian(30.0d0) )

    ! copy obj4 to obj5
    call obj5%copy(obj4)
    call obj5%resize(x=10.0d0,y=10.0d0,z=10.0d0)
    call obj5%gmsh(Name="obj5")

    ! remove obj5
    call obj5%remove()

    ! Create cylinder mesh
    call obj5%create(meshtype="Cylinder",x_num=10,y_num=10,z_num=10,x_len=5.0d0,y_len=50.0d0,z_len=10.0d0)
    call obj5%resize(x=10.0d0,y=10.0d0,z=10.0d0)
    call obj5%gmsh(Name="obj5_1")

    ! export as JSON file
    call obj5%json("obj5.json",endl=.true.)

end program main

```


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

-->
These are minor classes.

[StrainClass](https://github.com/kazulagi/plantfem/tree/master/src/StrainClass/StrainClass.f90)
[StressClass](https://github.com/kazulagi/plantfem/tree/master/src/StressClass/StressClass.f90)
[ConstitutiveModelClass](https://github.com/kazulagi/plantfem/tree/master/src/ConstitutiveModelClass/ConstitutiveModelClass.f90)
[FEMIfaceClass](https://github.com/kazulagi/plantfem/tree/master/src/FEMIfaceClass/FEMIfaceClass.f90)
[PostProcessingClass](https://github.com/kazulagi/plantfem/tree/master/src/PostProcessingClass/PostProcessingClass.f90)
