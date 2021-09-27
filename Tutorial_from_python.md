```<br> ```# Welcome from Python!


Here is a cheat sheet of plantFEM for python users.

Here is a typical plantFEM (.f90) script.

```fortran

use plantfem

implicit none

! COMMENT

! Declare variables used in the script.
integer(int32)::[variable_name]
real(real64)::[variable_name]
type([some struct-name or class-name])::[variable_name]
...

! Write what to do
...

end

```

| Question | Python (3.4 or later) | plantFEM (and/or Fortran) |
| ---- | ------ | ---- |
| How to execute a script? | ```python3 [script.py]``` | ```plantfem [script.f90]``` |
| How to compile a script? | - | ```plantfem build [script.f90]```,<br> then, an executable file<br> ```server.out``` is created. |
| Need declarations before using it? | No | Yes|
| How to call external modules/libraries | ```import [library_name]``` | ```use [library_name]``` |
| How to define a function which <br> has return values. | ```def func(arg): ```<br><tab>```　　retrun ret``` | ```function func(arg) result(ret) ```<br>``` ```<br>``` end function func``` |
| How to define a function which <br> does NOT have return values. | ```def sub(arg1, arg2, ...): ```<br>```　　return ret``` | ```subroutine sub(arg1, arg2,...)  ```<br>``` ```<br>``` end subroutine sub``` |
| Does indentation have any meaning? | Yes | No |
| Are there any statements that must <br> be written in a program? | No | Yes, it is ```implicit none```  |
| How to call a function with return values? | ```ret = func(arg)``` | ```ret = func(arg)``` |
| How to call a function WITHOUT return values? | ```func(arg)``` | ```call func(arg)``` |
|Should the declarations be written<br>  together at the beginning?　| No | Yes|
| How to call class-method <br> or menber variables | ```.``` | ```%```|
| How to comment-out | ``````<br> ```#``` | ```!```|


| Types in Python | Types in plantFEM |
| ---- | ---- |
| ```int``` |``` integer(int16)```,``` integer(int32)```,or ```integer(int64)```|
| ```float``` |``` real(real32)```,``` real(real64)```,or ```real(real128)``` |
| ```complex``` |``` complex```,``` double complex```,``` ...etc. |
| ```string``` |```character``` |

| How to write this (Basic operations) | ... in plantFEM?         | ...and what to declare before calling it?|
| :---- | :---- | :---- |
| ```for i in range(10): ```<br>```　　 ``` | ```do i=1, 10 ```<br>``` ```<br>``` enddo ``` |  ```integer(int32)::i``` |
| ```if a == b: ```<br>```　　 ``` | ```if(a==b)then ```<br>``` ```<br>``` end if ``` | - |
| ```if a != b: ```<br>```　　 ``` | ```if(a/=b)then ```<br>``` ```<br>``` end if ``` | - |
| ```if a > b: ```<br>```　　 ``` | ```if(a > b)then ```<br>``` ```<br>``` end if ``` | - |
| ```if a < b: ```<br>```　　 ``` | ```if(a < b)then ```<br>``` ```<br>``` end if ``` | - |
| ```if a < b and a > c: ```<br>```　　 ``` | ```if(a < b .and. a > c)then ```<br>``` ```<br>``` end if ``` | - |
| ```print(a) ``` |```call print(a)  ``` |- |
| ```print("hello "+"world!") ``` |```call print("hello "//"world!")  ``` |- |


| How to write this (File-IO) | ... in plantFEM?         | ...and what to <br> declare before calling it?|
| ---- | ---- | ---- |
| ```f = open("text.txt","w")``` | ```call f%open("text.txt","w")``` | ```type(IO_)::f``` |
| ```f.write("hello)"``` | ```call f%write("hello")``` or<br>  ```write(f%fh,*) "hello"```  | - |
| ```line = f.readline()"``` | ```line = f.readline()``` or<br>  ```read(f%fh,*) line```  | ```character(:),allocatable::line``` |
| ```int_value = int(line)``` |``` int_value = fint(line) ```| ```integer(int32)::int_value ```|
| ```real_value = float(line)``` |``` real_value = freal(line) ```| ```real(real64)::real_value ```|
| ```f.close("hello)"``` | ```call f%close()``` | - |


| How to write this (Numpy) | ... in plantFEM?         | ...and what to declare before calling it?|
| ---- | ---- | ---- |
| ```a = np.zeros(5)``` | ```a = zeros(5)``` | ```real(real64),allocatable::a(:)``` |
| ```a = np.linspace(2.0, 3.0, num=5)``` | ```a = linspace([2.0d0,3.0d0],5 )``` | ```real(real64),allocatable::a(:)``` |
| ```a = np.arange(3,7)``` | ```a = arange(3,7)``` | ```real(real64),allocatable::a(:)``` |
| ```a = np.array([2.0, 3.0, 4.0]) ``` | ```a = [2.0d0, 3.0d0, 4.0d0] ``` | ```real(real64),allocatable::a(:)``` |
| ```a = np.array([2.0, 3.0],[4.0,5.0]) ``` | ```a(1,1:2) = [2.0d0, 3.0d0]```<br> ```a(2,1:2) = [3.0d0, 4.0d0]```<br> ``` ``` | ```real(real64),allocatable::a(:,:)```<br> ``` a = zeros(2,2)``` |
| ```dp = np.dot(a,b) ```<br> ```# a is vector, b is vector``` | ```dp = dot_product(a,b)``` | ``` ``` |
| ```dp = np.dot(a,b) ```<br> ```# a is matrix, b is ```<br> ```# vector or matrix``` | ```dp = matmul(a,b)``` | ```real(real64),allocatable::dp(:) ! or dp(:,:)``` |
| ```a1 = a[0] ```<br> ```#index starts from 0 as default.``` | ```a1 = a(1) ! index starts from 1 as default.``` | - |
| ```random_value = np.random.rand()``` |```random_value = random.random()``` |```type(Random_)::random``` |
| ```random_value = np.random.normal(loc=average, scale=standard_deviation)``` |```random_value = random.gauss(mu=average, sigma=standard_deviation)``` |```type(Random_)::random``` |
| ```a = a.T ```<br> ```#transpose ``` |```a = transpose(a) ``` |- |
| ```a = reshape(a,2,2) ```<br> ```#reshape ``` |```a = reshape(a,2,2) ``` |- |

| What's this? | Explanation |
|:------------ |:------------ |
|allocatable | [English](https://www.ibm.com/docs/en/xl-fortran-aix/16.1.0?topic=attributes-allocatable-fortran-2003) <br> [Japanese](https://www.nag-j.co.jp/nagfor/np52_manual/np52_manual_10_3.html) <br>```allocatable``` is one of the strongest feature of the Fortran. We can create any dynamic arrays of any types, including real, integer, character, and derived data types. |
|intent | [English](https://pages.mtu.edu/~shene/COURSES/cs201/NOTES/chap07/intent.html) <br> [Japanese](https://www.nag-j.co.jp/fortran/FI_11.html) |

Other features are presented in [Documentation](../Tutorial_std.md)

