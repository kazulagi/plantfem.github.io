[plantFEM](https://kazulagi.github.io/plantfem.github.io) >> [std](https://kazulagi.github.io/plantfem.github.io/Tutorial_std.html)

# plantFEM std library

Welcome to a tutorial for plantFEM std library.

The std library contains following classes and modules ([std](https://github.com/kazulagi/plantfem/tree/master/src/std/std.f90)).

## modules

Followings are major classes.

### (1) plantFEM uses [iso_fortran_env](https://gcc.gnu.org/onlinedocs/gfortran/ISO_005fFORTRAN_005fENV.html) for basic data types. 

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

### (2) [TimeClass](https://github.com/kazulagi/plantfem/tree/master/src/TimeClass/TimeClass.f90) is a class for time measurement, which can be used to measure cpu time by creating an instance of type time_.

Ex. [Example1](https://github.com/kazulagi/plantfem/tree/master/Tutorial/playon_std/ex0000_time.f90)


```fortran

program main
    use std
    implicit none

    type(Time_) :: time

    ! stop-watch starts!
    call time%start()
    
    print *, "Hello!"
    
    ! measuring cpu-time.
    call time%show()

    ! reset stop-watch
    call time%reset()

    print *, "How are you today?"
    
    ! measuring cpu-time.
    call time%show()

    ! sleep for 20 sec.
    call time%sleep(20)

end program main

```



If the script is named as "server.f90" and located in the root repository of the plantfem, the script is executable by this command.

```shellscript

./plantfem run

```


The result of the script is,

```shellscript

 Hello!
   2.3000000000000017E-005
 How are you today?
   2.4729999999999999E-003

```


### (3) [MathClass](https://github.com/kazulagi/plantfem/tree/master/src/MathClass/MathClass.f90) is a library for mathematical operations such as matrix calculation and tensor operations. Although "Class" is in the name, but this is just a toolbox of useful functions.


Ex.


```fortran

program main
    use plantFEM
    implicit none


    real(real64) :: re64=100.0d0
    real(real64) :: vec(100)=1.0d0
    real(real64) :: vec3(3)=1.0d0
    real(real64) :: tensor(3,3)=0.0d0

    print *, "100 radian = ", degrees(re64),"degrees"
    print *, "100 degrees= ", radian(re64),"radian"

    print *, "real64 => string: ", str(re64)

    print *, "norm of vec(100) : ", norm(vec)

    tensor(:,:) = tensor_product(vec3,vec3)

    print *, tensor(1,:)
    print *, tensor(2,:)
    print *, tensor(3,:)

    tensor(:,:)=0.0d0

    tensor(1,1)=1.0d0
    tensor(2,2)=1.0d0
    tensor(3,3)=1.0d0

    print *, "det(tensor) : ", det_mat(tensor,size(tensor,1) )

    print *, "...etc."
    
end program main

```



If the script is named as "server.f90" and located in the root repository of the plantfem, the script is executable by this command.

```shellscript

./plantfem run

```


The result of the script is,

```shellscript

 100 radian =    5729.5779514719952      degrees
 100 degrees=    1.7453292519444445      radian
 real64 => string: 100.00000000        
 norm of vec(100) :    10.000000000000000     
   1.0000000000000000        1.0000000000000000        1.0000000000000000     
   1.0000000000000000        1.0000000000000000        1.0000000000000000     
   1.0000000000000000        1.0000000000000000        1.0000000000000000     
 det(tensor) :    1.0000000000000000     
 ...etc.

```

### (4) [IOClass](https://github.com/kazulagi/plantfem/tree/master/src/IOClass/IOClass.f90) is a class to realize a file operation like Python in Fortran. You can create an instance of type "IO_" for easy file operation.


Ex.


```fortran

program main
    use std ! standard package of SiCroF library
    implicit none

    ! This example utilizes Input-Output Class, where
    ! You can handle external files.

    ! How to use:

    ! First, create the instance

    type(IO_) :: f ! file-IO instance
    integer(int32) :: i, num ! int i and int num
    

    ! #1 create, edit and close files.
    ! ----> open file(filepath, filename, extention)
    call f%open("./","test",".txt")
    ! write something
    call f%write(str(100.0d0) )
    write(f%fh,*) 100.0d0
    ! and close it
    call f%close()



    ! ----> create sequential files (filepath, filename, extention)
    ! it creates
    ! ./hello1.txt
    ! ./hello2.txt
    ! ./hello3.txt
    ! ...
    ! ./hello10.txt

    ! for i=1; i<11;i++
    do i=1,10
        !    f.open(filepath, filename, extention)
        !    str(int) => string
        call f%open("./","hello"//trim(str(i)),".txt")
        
        ! This
        call f%write(str(i))
        ! and this
        write(f%fh,*) str(i)
        ! are same 

        call f%close()

        call f%open("./","hello"//trim(str(i)),".txt")        
        ! read a line
        read(f%fh,*) num
        ! print(num)
        print *, num

        call f%close()
    enddo

    
end program 

```



If the script is named as "server.f90" and located in the root repository of the plantfem, the script is executable by this command.

```shellscript

./plantfem run

```


The result of the script is,

```shellscript

           1
           2
           3
           4
           5
           6
           7
           8
           9
          10

```

### (5) [RandomClass](https://github.com/kazulagi/plantfem/tree/master/src/RandomClass/RandomClass.f90) is a class in Fortran to do the same thing as the random class in Python that does pseudo-random number generation.

Ex >> Next section

### (6) [ArrayClass](https://github.com/kazulagi/plantfem/tree/master/src/ArrayClass/ArrayClass.f90) is a toolbox of functions and subroutines that perform array operations, similar to Python's numpy.


Ex.


```fortran

program main
    use std ! standard package of SiCroF library
    implicit none

    ! This example utilizes Array Class, where
    ! You can handle external files.

    ! How to use:

    ! First, create the instance    
    real(real64),allocatable :: a(:,:)
    real(real64),allocatable :: b(:,:)
    real(real64),allocatable :: ab(:,:)
    real(real64),allocatable :: copyobj(:,:)
    real(real64),allocatable :: ret(:,:)
    integer(int32) :: i

    type(IO_) :: f

    ! #1 create array
    ! ----> allocate array
    allocate(ret(3,3) )
    ! ----> set identity matrix
    ret = identity_matrix(3)
    ! show array on terminal.
    print *, "ret = "
    call showArray(ret)

    ! #2 Array Input-Output
    ! ----> save matrix as "./test.txt" file
    call savetxt(ret,"./","test",".txt")
    ! ----> load a .txt or .csv file.
    a = loadtxt("./","test",".txt")
    b = loadtxt("./","test",".txt")

    ! -----> also, you can do it for multiple arrays as
    call f%open("./","arrays",".txt")
    ! save "a" array
    call saveArray(f%fh,a)
    ! save "b" array
    call saveArray(f%fh,b)
    ! save "ret" array
    call saveArray(f%fh,ret)
    call f%close()

    ! -----> and,
    call f%open("./","arrays",".txt")
    ! load "a" array
    call loadArray(f%fh,a)
    ! load "b" array
    call loadArray(f%fh,b)
    ! load "ret" array
    call loadArray(f%fh,ret)
    call f%close()


    ! -----> It shows the array.
    print *, "a = "
    call showArray(a)

    ! -----> copy array
    a(1,2)=20.0d0
    call copyarray(a,copyobj)
    ! -----> It shows the array.
    print *, "a (original) = "
    call showArray(a)
    print *, "copy = "
    call showArray(copyobj)

    ! Importance Index 7 / 10 : [*******   ]

    
end program 

```



If the script is named as "server.f90" and located in the root repository of the plantfem, the script is executable by this command.

```shellscript

./plantfem run

```


The result of the script is,

```shellscript

ret = 
   1.0000000000000000        0.0000000000000000        0.0000000000000000     
   0.0000000000000000        1.0000000000000000        0.0000000000000000     
   0.0000000000000000        0.0000000000000000        1.0000000000000000     
 a = 
   1.0000000000000000        0.0000000000000000        0.0000000000000000     
   0.0000000000000000        1.0000000000000000        0.0000000000000000     
   0.0000000000000000        0.0000000000000000        1.0000000000000000     
 a (original) = 
   1.0000000000000000        20.000000000000000        0.0000000000000000     
   0.0000000000000000        1.0000000000000000        0.0000000000000000     
   0.0000000000000000        0.0000000000000000        1.0000000000000000     
 copy = 
   1.0000000000000000        20.000000000000000        0.0000000000000000     
   0.0000000000000000        1.0000000000000000        0.0000000000000000     
   0.0000000000000000        0.0000000000000000        1.0000000000000000    
```

### (7) [GraphClass](https://github.com/kazulagi/plantfem/tree/master/src/GraphClass/GraphClass.f90) is a class for implementing the graph structure and performing operations. Implementation in progress.

### (8) [MPIClass](https://github.com/kazulagi/plantfem/tree/master/src/MPIClass/MPIClass.f90) is class as a wrapper of MPI in terms of Fortran. You can easily perform MPI parallelization by using its instance.

Ex.


```fortran

program main
    use std ! use standard libary of SiCroF
    implicit none
    ! start
    type(MPI_) :: mpid
    type(IO_) :: f
    integer(int32) :: fh ! file handle (id)
    integer(int32) :: val, from
    integer(int32) :: send_value(2)
    integer(int32),allocatable :: recv_values(:)

    ! start MPI
    call mpid%start()
    ! >>>>>>>> do parallel

    ! hello, world
    print *, "hello! world, my rank is ", mpid%myrank



    ! create files
    call f%open("./","FileFromRank"//trim(str(mpid%myrank)),".txt")
    write(f%fh,*) "MPI is running!"
    write(f%fh,*) "My rank is ",mpid%myrank
    write(f%fh,*) "Number of process is ",mpid%petot
    call f%close()






    ! ----> broadcast (sync) one data => to all nodes.
    val = mpid%myrank
    print *, "my value :: ",val, "my rank is : ", mpid%myrank
    ! broadcast => sync "val" of rank "0" node to all!
    from = 0
    call mpid%bcast(From=from,val=val)
    print *, "Sync! >> my value :: ",val, "my rank is : ", mpid%myrank





    ! ----> gather all values to rank 0 node
    allocate(recv_values(mpid%petot*2) )
    send_value(:) = mpid%myrank
    recv_values(:) = 0
    ! gether send_value of all nodes => to "recv_value(:) "
    call mpid%gather(sendobj=send_value(1:2),&
        recvobj=recv_values(1:),To=0)
    if(mpid%myrank == 0)then
        print *, "Gathered values : "
        print *, recv_values(:)
    endif




    ! Wait all process 
    call mpid%barrier()
    ! <<<<<<<< end do parallel
    call mpid%end()

    ! Importance Index 6 / 10 : [******    ]

end program 

```



If the script is named as "server.f90" and located in the root repository of the plantfem, the script is executable by this command.

```shellscript

./plantfem run

```


The result of the script is,

```shellscript


 Number of Core is            1
 hello! world, my rank is            0
 my value ::            0 my rank is :            0
 Sync! >> my value ::            0 my rank is :            0
 Gathered values : 
           0           0
  ############################################ 
  Computation time (sec.) ::     2.4629099999999999E-004
  Number of cores         ::             1
  ############################################ 

```

### (9) In [DictionaryClass](https://github.com/kazulagi/plantfem/tree/master/src/DictionaryClass/DictionaryClass.f90), we implemented the Python equivalent of a dictionary class. To be honest, I don't use it much.


### (10) [OpenMPClass](https://github.com/kazulagi/plantfem/tree/master/src/OpenMPClass/OpenMPClass.f90) is class as a wrapper of OpenMP in terms of Fortran. [MPIClass](https://github.com/kazulagi/plantfem/tree/master/src/MPIClass/MPIClass.f90) is very good and useful, but this class is honestly a lost cause and needs a vision.


### (11) [LinearSolverClass](https://github.com/kazulagi/plantfem/tree/master/src/LinearSolverClass/LinearSolverClass.f90) is a solver for linear equations. It is an original implementation. It is planned to be replaced in the future by the well-known MKL and LAPACK.

Ex.



```fortran

program main
    use LinearSolverClass
    implicit none

    ! Create Ax=B and solve x = A^(-1) B
    integer(int32), parameter :: N = 4 ! rank = 3
    real(real64)  :: t1,t2 ! time measure
    real(real64) :: A(1:N,1:N), B(1:N), X(1:N) ! A, x and B
    real(real64) :: Val(1:10) 
    integer(int32) :: index_i(1:10),index_j(1:10)
    type(LinearSolver_) :: solver ! linear solver instance.

    ! creating A, x, and B
    A(1,1:4) = (/  2.0d0, -1.0d0,  0.0d0, 0.0d0 /)
    A(2,1:4) = (/ -1.0d0,  2.0d0, -1.0d0, 0.0d0 /)
    A(3,1:4) = (/  0.0d0,  2.0d0,  3.0d0, 1.0d0 /)
    A(4,1:4) = (/  0.0d0,  0.0d0,  1.0d0, 2.0d0 /)

    X(1:4)   = (/  0.0d0,  0.0d0,  0.0d0, 0.0d0 /)

    B(1:4)   = (/  1.0d0,  2.0d0,  3.0d0, 4.0d0 /)

    ! CRS-format
    Val(1:10) = (/  2.0d0, -1.0d0,  -1.0d0,  2.0d0, -1.0d0, 2.0d0, 3.0d0, &
        1.0d0, 1.0d0,  2.0d0/)
    index_i(1:10) = (/ 1, 1, 2, 2, 2, 3, 3, 3, 4, 4/)
    index_j(1:10) = (/ 1, 2, 1, 2, 3, 2, 3, 4, 3, 4/)

    ! import Ax=B into the linear solver instance.
    call solver%import(a = A, x = X, b = B)

    ! get a cpu-time
    call cpu_time(t1)  
    ! solve Ax=B by Gauss-Jordan
    call solver%solve(Solver="GaussJordan")
    ! get a cpu-time
    call cpu_time(t2)  
    ! show result
    ! X is stored in solver%X(:) (solver.x[])
    print *, "Solved by GaussJordan",solver%X(:),"/",t2-t1," sec."

    ! Similarly ...

    X(1:4)   = (/  0.0d0,  0.0d0,  0.0d0, 0.0d0 /)
    call solver%import(a = A, x = X, b = B, val=val, index_i=index_i, index_j=index_j)
    call cpu_time(t1)  
    call solver%solve(Solver="GaussSeidel")
    call cpu_time(t2)  
    print *, "Solved by GaussSeidel",solver%X(:),"/",t2-t1," sec."
    
    X(1:4)   = (/  0.0d0,  0.0d0,  0.0d0, 0.0d0 /)
    call solver%import(a = A, x = X, b = B, val=val, index_i=index_i, index_j=index_j)
    call cpu_time(t1)  
    call solver%solve(Solver="BiCGSTAB",CRS=.true.)
    call cpu_time(t2)  
    print *, "Solved by BiCGSTAB(CRS)",solver%X(:),"/",t2-t1," sec."

    X(1:4)   = (/  0.0d0,  0.0d0,  0.0d0, 0.0d0 /)
    call solver%import(a = A, x = X, b = B, val=val, index_i=index_i, index_j=index_j)
    call cpu_time(t1)  
    call solver%solve(Solver="BiCGSTAB",CRS=.false.)
    call cpu_time(t2)  
    print *, "Solved by BiCGSTAB   ",solver%X(:),"/",t2-t1," sec."

    X(1:4)   = (/  0.0d0,  0.0d0,  0.0d0, 0.0d0 /)
    call solver%import(a = A, x = X, b = B)
    call cpu_time(t1)  
    call solver%solve(Solver="GPBiCG")
    call cpu_time(t2)  
    print *, "Solved by GPBiCG     ",solver%X(:),"/",t2-t1," sec."
    
    ! Importance Index 9 / 10 : [********* ]
    
end program

```

If the script is named as "server.f90" and located in the root repository of the plantfem, the script is executable by this command.

```shellscript

./plantfem run

```


The result of the script is,

```shellscript


 Solved by GaussJordan   1.1304347826086956        1.2608695652173914      -0.60869565217391308        2.3043478260869565      /   1.4000000000000123E-005  sec.
 Solved by GaussSeidel   1.1304329905708359        1.2608637700010377      -0.60869342176215169        2.3043467108810760      /   1.3500000000000014E-004  sec.
 Solved by BiCGSTAB(CRS)   1.1304347826086956        1.2608695652173909      -0.60869565217391319        2.3043478260869570      /   1.1999999999999858E-005  sec.
 Solved by BiCGSTAB      1.1304347826086953        1.2608695652173911      -0.60869565217391308        2.3043478260869565      /   9.9999999999995925E-006  sec.
 Solved by GPBiCG        1.1304348100123531        1.2608695899884879      -0.60869565841905671        2.3043478274256657      /   3.4999999999999875E-005  sec.

```





### (12) [TreeClass](https://github.com/kazulagi/plantfem/tree/master/src/TreeClass/TreeClass.f90) is a class for tree graphs. The maintenance is currently suspended. This class is looking for a vision.

### (13) [ShapeFunctionClass](https://github.com/kazulagi/plantfem/tree/master/src/ShapeFunctionClass/ShapeFunctionClass.f90) is a class for the shape function. Currently we only use 2D 4-node iso-parametric elements and 3D 8-node iso-parametric elements. We are looking for committers to implement other types of elements.



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

[HTTPClass](https://github.com/kazulagi/plantfem/tree/master/src/HTTPClass/HTTPClass.f90) is a remnant from when I was trying to set up an http server at plantFEM. It is waiting for your commit.

