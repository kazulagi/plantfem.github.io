[plantFEM](https://kazulagi.github.io/plantfem.github.io) >> [obj](https://kazulagi.github.io/plantfem.github.io/Tutorial_obj.html)

# plantFEM fem library

Welcome to a tutorial for plantFEM fem library.

The fem library contains following classes and modules ([fem](https://github.com/kazulagi/plantfem/tree/master/src/fem/fem.f90)).

## modules

Followings are major classes.


### (1) [DiffusionEquationClass](https://github.com/kazulagi/plantfem/tree/master/src/DiffusionEquationClass/DiffusionEquationClass.f90)

Sample code (Tutorial/obj/ex0003_diffusion3D.f90):

```Fortran

program main
    use sim
    implicit none

    type(FEMDomain_),target :: domain
    type(MaterialProp_) :: Permiability
    type(Boundary_) :: Dirichlet,Initial1,Initial2
    type(DiffusionEq_) :: Solver
    integer(int32) :: i

    call domain%create(meshtype="rectangular3D",x_num=20,y_num=20,z_num=10,&
        x_len=50.0d0,y_len=50.0d0,z_len=10.0d0)

    !call domain%create(meshtype="Sphere3D",x_num=10,y_num=10,z_num=10,&
    !    x_len=50.0d0,y_len=20.0d0,z_len=10.0d0)
    !call domain%gmsh(Name="test")

    call Permiability%create(Name="Permiability",ParaValue=0.10d0,Layer=1) 
    call Dirichlet%create(Category="Dirichlet",BoundValue=1.0d0,x_max=1.0d0,x_min=-1.0d0,Layer=1)
    call Dirichlet%create(Category="Dirichlet",BoundValue=5.0d0,x_max=51.0d0,x_min=49.0d0,Layer=1)
    call Initial2%create(Category="Time",BoundValue=-10.0d0,x_max=20.0d0,x_min=40.0d0,z_min=7.0d0,Layer=1)

    call domain%import(Materials=.true. , Material=Permiability)
    call domain%import(Boundaries=.true. , Boundary=Dirichlet)
    !call domain%import(Boundaries=.true. , Boundary=Initial1)
    call domain%import(Boundaries=.true. , Boundary=Initial2)

    call domain%bake(template="DiffusionEq_" )

    solver%FEMDomain => domain

    solver%dt=1.0d0
    
    call solver%setup()
    call solver%solve(solvertype="BiCGSTAB")
    call solver%display(Name="res",optionalstep=0,displaymode="gmsh")
    do i=1,10
        call solver%update()
        call solver%solve(solvertype="BiCGSTAB")
        call solver%display(Name="res",optionalstep=i,displaymode="gmsh")
    enddo

end program main

```


### (2) [FiniteDeformationClass](https://github.com/kazulagi/plantfem/tree/master/src/FiniteDeformationClass/FiniteDeformationClass.f90)


Sample code (Tutorial/obj/ex0004_Deformation3D.f90):

```Fortran
program main
    use sim
    implicit none

    type(FEMDomain_),target :: domain
    type(MaterialProp_) :: YoungsModulus, PoissonRatio,Density,Cohesion,Phi,psi
    type(Boundary_) :: disp_x,disp_y,disp_z
    type(IO_) :: f
    type(FiniteDeform_) :: Solver
    integer(int32) :: i

    call domain%create(meshtype="rectangular3D",x_num=10,y_num=10,z_num=10,&
        x_len=50.0d0,y_len=20.0d0,z_len=10.0d0)

    !call domain%create(meshtype="Sphere3D",x_num=10,y_num=10,z_num=10,&
    !    x_len=50.0d0,y_len=20.0d0,z_len=10.0d0)
    !call domain%gmsh(Name="test")

    call YoungsModulus%create(Name="YoungsModulus",ParaValue=100.0d0,Layer=1) 
    call PoissonRatio%create(Name="PoissonRatio",ParaValue=0.20d0,Layer=2) 
    call Density%create(Name="Density",ParaValue=0.00d0,Layer=3)
    call Cohesion%create(Name="Cohesion",ParaValue=100000000.0d0,Layer=4)
    call Phi%create(Name="Psi",ParaValue=0.0d0,Layer=5)
    call psi%create(Name="psi",ParaValue=0.0d0,Layer=6)

    call disp_x%create(Category="Dirichlet",BoundValue=0.0d0,x_max=1.0d0,x_min=-1.0d0,Layer=1)!x
    call disp_y%create(Category="Dirichlet",BoundValue=0.0d0,x_max=1.0d0,x_min=-1.0d0,Layer=2)!y
    call disp_z%create(Category="Dirichlet",BoundValue=0.0d0,x_max=1.0d0,x_min=-1.0d0,Layer=3)!z
    call disp_x%create(Category="Dirichlet",BoundValue=5.0d0,x_max=51.0d0,x_min=40.0d0,Layer=1)

    call domain%import(Materials=.true., Material=YoungsModulus)
    call domain%import(Materials=.true., Material=PoissonRatio)
    call domain%import(Materials=.true., Material=Density)
    call domain%import(Materials=.true., Material=Cohesion)
    call domain%import(Materials=.true., Material=Phi)
    call domain%import(Materials=.true., Material=Psi)

    call domain%import(Boundaries=.true. , Boundary=disp_x)
    call domain%import(Boundaries=.true. , Boundary=disp_y)
    call domain%import(Boundaries=.true. , Boundary=disp_z)

    call domain%bake(template="FiniteDeform_")

    solver%FEMDomain => domain

    solver%dt=1.0d0
    solver%infinitesimal = .true.

    call solver%DivideBC()
    call solver%solve(solvertype="BiCGSTAB")
    do i=1,10
        call solver%UpdateInitConfig()
        call solver%UpdateBC()
        call solver%solve(solvertype="BiCGSTAB")
        call solver%display(DisplayMode="gmsh",OptionalStep=i,Name="deform")
    enddo

end program main
```

### (3) [ContactMechanicsClass(experimental)](https://github.com/kazulagi/plantfem/tree/master/src/ContactMechanicsClass/ContactMechanicsClass.f90)

Sample code (Tutorial/sim/Contact3DLinearElastic.f90):

```Fortran
use ContactMechanicsClass
implicit none

type(FEMDomain_),allocatable :: domains(:)
type(ContactMechanics_) :: contact
integer(int32),allocatable :: FixBoundary(:)
integer(int32) :: contactList(3,3)=0

allocate(domains(3) )
! contact of three cubes
call domains(1)%create(meshtype="Cube3D",y_num=3,z_num=3)
call domains(1)%resize(x=2.0d0,y=2.0d0,z=2.0d0)

call domains(2)%create(meshtype="Cube3D",y_num=3,z_num=3)
call domains(2)%resize(x=2.0d0,y=2.0d0,z=2.0d0)
call domains(2)%move(x=1.9d0)

call domains(3)%create(meshtype="Cube3D",y_num=3,z_num=3)
call domains(3)%resize(x=2.0d0,y=2.0d0,z=2.0d0)
call domains(3)%move(x=3.9d0)

! contact domain#1 => domain#2
contactList(1,2) = 1
! contact domain#2 => domain#3
contactList(2,3) = 1
! initialize simulator
call contact%init(femdomains=domains,contactlist=contactlist)

! change density
call contact%setDensity(density=0.10d0,DomainID=1)
call contact%setDensity(density=0.10d0,DomainID=2)
call contact%setDensity(density=0.10d0,DomainID=3)

!check properties
call contact%showProperty()

! setup solver
call contact%setup(penaltyparameter=500.0d0)
! fix displacement
call contact%fix(direction="x",disp= 0.0d0, DomainID=1,x_max=0.10d0)
call contact%fix(direction="y",disp= 0.0d0, DomainID=1,x_max=0.10d0)
call contact%fix(direction="z",disp= 0.0d0, DomainID=1,x_max=0.10d0)
call contact%fix(direction="x",disp= 0.0d0, DomainID=3,x_min=5.80d0)
call contact%fix(direction="y",disp= 0.0d0, DomainID=3,x_min=5.80d0)
call contact%fix(direction="z",disp=-0.50d0,DomainID=3,x_min=5.80d0)

! solve > get displacement
call contact%solver%solve("BiCGSTAB")
! update mesh
call contact%updateMesh()
! show results
call domains(1)%msh("domains(1)_result")
call domains(2)%msh("domains(2)_result")
call domains(3)%msh("domains(3)_result")

end
```


### (4) [SeismicAnalysisClass(experimental)](https://github.com/kazulagi/plantfem/tree/master/src/SeismicAnalysisClass/SeismicAnalysisClass.f90)

Sample code (Tutorial/sim/SeismicAnalysis.f90):

```Fortran
use sim
implicit none

type(SeismicAnalysis_) :: seismic(100)
type(FEMDomain_),target :: cube,original
type(IO_) :: f,response,history_A,history_V,history_U,input_wave
type(Math_) :: math
type(MPI_) :: mpid
real(real64),allocatable :: disp_z(:,:)
real(real64) :: wave(1000,2),T,Duration,dt
integer(int32) :: i,j,cases,stack_id,num_of_cases

num_of_cases = 4

call mpid%start()
call mpid%createStack(num_of_cases)


! create Domain
call cube%create(meshtype="Cube3D",x_num=2,y_num=2,z_num=20)
call cube%resize(x=1.0d0,y=1.0d0,z=5.0d0)
call cube%move(z=-5.0d0)

! Change T = 0.1, 0.2, 0.3 ... 1 (sec.)

do stack_id=1,size(mpid%localstack)
    cases = mpid%localstack(stack_id)
    ! create Wave
    T = 0.010d0*cases+0.50d0 ! sec.
    Duration = T * 10.0d0 ! sec.
    dt = Duration/dble(size(wave,1))/10.0d0
    
    wave(:,:) = 0.0d0
    do i=1,size(wave,1)
        wave(i,1) = dt*dble(i)
        wave(i,2) = sin(2.0d0*math%pi/T*wave(i,1) )
    enddo
    call input_wave%open("input_wave_"//str(cases)//".txt" )
    call input_wave%write(wave)
    call input_wave%close()
    original = cube
    

    ! set domain
    seismic(cases)%femdomain => cube
    ! set wave
    seismic(cases)%wave = wave
    seismic(cases)%dt = dt
    ! run simulation
    call seismic(cases)%init()
    call seismic(cases)%fixDisplacement(z_max = -4.99d0,direction="x")
    call seismic(cases)%fixDisplacement(z_max = -4.99d0,direction="y")
    call seismic(cases)%fixDisplacement(z_max = -4.99d0,direction="z")

    call seismic(cases)%fixDisplacement(y_max = 0.0d0,direction="y")
    call seismic(cases)%fixDisplacement(y_min = 1.0d0,direction="y")

    call seismic(cases)%fixDisplacement(direction="z")

    call seismic(cases)%femdomain%vtk("mesh"//str(cases)//"_" )

    call seismic(cases)%loadWave(z_max=-4.50d0,direction="x",wavetype=WAVE_ACCEL)

    seismic(cases)%Density(:)      = 17000.0d0 !(N/m/m/m)
    seismic(cases)%YoungModulus(:) = 7000000.0d0 !(N/m/m)
    seismic(cases)%PoissonRatio(:) = 0.40d0 

    !seismic(cases)%alpha = 0.0d0
    !seismic(cases)%beta = 0.0d0

    call history_A%open("history_A"//str(cases)//".txt","w")
    call history_V%open("history_V"//str(cases)//".txt","w")
    call history_U%open("history_U"//str(cases)//".txt","w")
    
    do i=1,999
        call seismic(cases)%run(timestep=[i,i+1],AccelLimit=10.0d0**8)
        write(history_A%fh,*) real(dble(i)*dt), real(seismic(cases)%A( size(seismic(cases)%A)-2 ))
        write(history_V%fh,*) real(dble(i)*dt), real(seismic(cases)%V( size(seismic(cases)%V)-2 ))
        write(history_U%fh,*) real(dble(i)*dt), real(seismic(cases)%U( size(seismic(cases)%U)-2 )    )
        call history_A%flush()
        call history_V%flush()
        call history_U%flush()
    enddo
    call history_A%close()
    call history_V%close()
    call history_U%close()

    print *, maxval(seismic(cases)%U)


    seismic(cases)%femdomain%mesh%nodcoord = seismic(cases)%femdomain%mesh%nodcoord + (10.0d0**6)*&
        reshape(seismic(cases)%U, seismic(cases)%femdomain%nn(),seismic(cases)%femdomain%nd() )

    call seismic(cases)%femdomain%vtk("x10"//str(cases)//"_")

    call response%open("T_A"//str(cases)//".txt")
    call response%write(T,seismic(cases)%maxA(1))
    call response%close()

enddo

call mpid%end()

end

```