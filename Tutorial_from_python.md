# Welcome from Python!


Here is a cheat sheet of plantFEM for python users.

Here is a typical plantFEM (.f90) script.

```fortran

use plantfem

implicit none

! COMMENT

! Declare variables used in the script.
integer(int32) :: [variable_name]
real(real64) :: [variable_name]
type([some struct-name or class-name]) :: [variable_name]
...

! Write what to do
...

end

```

| Question | Python | plantFEM (and/or Fortran 2003) |
| ---- | ---- | ---- |
| Need definition before using it? | No | Yes|
| How to call external modules/libraries | import [library_name] | use [library_name] |
| How to define a function which has return values. | ```def func(arg): ~  retrun ret``` | ```function func(arg) result(ret) ~ end function func``` |
| How to define a function which does NOT have return values. | ```def sub(arg1, arg2, ...): ~ return ret``` | ```subroutine sub(arg1, arg2,...)  ~ end subroutine sub``` |
| Does indentation have any meaning? | Yes | No |
| Are there any statements that must be written in a program? | No | Yes, it is ```implicit none```  |
| How to call a function with return values? | ```ret = func(arg)``` | ```ret = func(arg)``` |
| How to call a function WITHOUT return values? | ```func(arg)``` | ```call func(arg)``` |
|Should the declarations be written together at the beginning?　| No | Yes|
| How to call class-method or menber variables | ```.``` | ```%```|
| How to comment-out | ```#``` | ```!```|


| Types in Python | Types in plantFEM |
| ---- | ---- |
| ```int``` |``` integer(int16)```,``` integer(int32)```,or ```integer(int64)```|
| ```float``` |``` real(real32)```,``` real(real64)```,or ```real(real128)``` |
| ```complex``` |``` complex```,``` double complex```,``` ...etc. |
| ```string``` |```character``` |

| How to write this (Basic operations) | ... in plantFEM?         | ...and what to declare before calling it?|
| ---- | ---- | ---- |
| ```for i in range(10): ~ ``` | ```do i=1, 10 ~ enddo ``` |  ```integer(int32) :: i``` |
| ```if a == b: ~ ``` | ```if(a==b)then ~ end if ``` | - |
| ```if a != b: ~ ``` | ```if(a/=b)then ~ end if ``` | - |
| ```if a > b: ~ ``` | ```if(a > b)then ~ end if ``` | - |
| ```if a < b: ~ ``` | ```if(a < b)then ~ end if ``` | - |
| ```if a < b and a > c: ~ ``` | ```if(a < b .and. a > c)then ~ end if ``` | - |



| How to write this (File-IO) | ... in plantFEM?         | ...and what to declare before calling it?|
| ---- | ---- | ---- |
| ```f = open("text.txt","w")``` | ```call f%open("text.txt","w")``` | ```type(IO_) :: f``` |
| ```f.write("hello)"``` | ```call f%write("hello")``` or ```write(f%fh,*) "hello"```  | - |
| ```line = f.readline()"``` | ```line = f.readline()``` or ```read(f%fh,*) line```  | ```character(:),allocatable :: line``` |
| ```int_value = int(line)``` |``` int_value = fint(line) ```| ```integer(int32) :: int_value ```|
| ```real_value = float(line)``` |``` real_value = freal(line) ```| ```real(real64) :: real_value ```|
| ```f.close("hello)"``` | ```call f%close()``` | - |




