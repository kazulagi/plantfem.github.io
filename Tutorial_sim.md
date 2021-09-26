[plantFEM](https://kazulagi.github.io/plantfem.github.io) >> [sim](https://kazulagi.github.io/plantfem.github.io/Tutorial_sim.html)


# plantFEM fem library

Welcome to a tutorial for plantFEM fem library.

The fem library contains following classes and modules ([fem](https://github.com/kazulagi/plantfem/tree/master/src/fem/fem.f90)).

## modules

Followings are major classes.


### (1) [SoybeanClass](https://github.com/kazulagi/plantfem/tree/master/src/SoybeanClass/SoybeanClass.f90) 


A simple sample code (Tutorial/obj/createSoybean.f90):

```Fortran
use SoybeanClass
implicit none

type(Soybean_) :: soy

call soy%init(config="Tutorial/obj/realSoybeanConfig.json")
call soy%stl(name="soy")

! measuring volume (m^2)
call print(soy%getVolume())

end
```


Other Sample code (Tutorial/obj/createAndRemoveSoybeanMPI.f90):

```Fortran
program main
    use SoybeanClass
    implicit none

    integer(int32),parameter :: num_para = 10

    type(MPI_) :: mpid
    type(IO_) :: f
    type(Soybean_) :: soy
    integer(int32) :: i

    call mpid%start()
    call f%open("SoyVolume_rank_"//str(mpid%myrank)//".txt","w")
    do i=1,num_para
        call soy%init(config="Tutorial/obj/realSoybeanConfig.json")
        call f%write(soy%getVolume(leaf=.true.,stem=.true.))
        call f%flush()
        call soy%remove()
    enddo
    call f%close()
    call mpid%end()

end program main
```


### (2) [MaizeClass](https://github.com/kazulagi/plantfem/tree/master/src/MaizeClass/MaizeClass.f90) 

Sample code (Tutorial/obj/createMaize.f90):


```Fortran
use MaizeClass
implicit none

type(Maize_) :: maize

call maize%create(config="Tutorial/obj/realMaizeConfig.json")
call maize%vtk("maize")

end
```



### (3) [GrapeClass](https://github.com/kazulagi/plantfem/tree/master/src/GrapeClass/GrapeClass.f90) 

Sample code (Tutorial/obj/createGrape.f90):


```Fortran
use GrapeClass
implicit none

type(Grape_) :: grape

call grape%create(config = "Tutorial/obj/realGrapeConfig.json")
call grape%vtk("grape")
call grape%stl("grape")

end
```




