---
layout: post
title: Using LAPACK libraries with Intel Fortran and Abaqus user subroutine
date: 2024-05-09 08:00:00-0400
description: A simple tutorial to compile and link LAPACK libraries with Intel Fortran compiler 
tags: Intel-oneAPI Abaqus user-subroutine Fortran programming Visual-Studio LAPACK Finite-element
categories: tutorial
giscus_comments: false
related_posts: true
# toc:
#   sidebar: left
---


## What is Linear Algebra PACKage (LAPACK)

Linear Algebra PACKage (LAPACK) is a Fortran-based efficient library for linear algebraic operations shared under a 3-Clause BSD License. Original LAPACK was written in FORTRAN 77 but later moved to Fortran 90. Many programming languages have interfaces or wrappers for the LAPACK library. The original LAPACK is hosted on [Netlib](https://www.netlib.org/lapack/), but many software companies have their own implementations of LAPACK such as Intel has its own version of LAPACK included in the Math Kernel Library (MKL) as a part of the Intel oneAPI package and Apple has its LAPACK library within the Accelerate framework. There are also simplified Fortran 95 interface libraries to the original LAPACK however it is not standardized and interfaces vary among different implementations. With LAPACK, you do not need to write standard linear algebraic procedures. See the [Netlib LAPACK documentation](https://www.netlib.org/lapack/explore-html/) to see what type of subroutines are available in LAPACK. The primary objective of this blog is to test the LAPACK library from Intel MKL with Abaqus user subroutines.


## Installing Visual Studio and Intel oneAPI

To use LAPACK subroutines from the Intel MKL in your Fortran codes, first, you need to have Microsoft Visual Studio and Intel oneAPI Toolkits (Base and HPC) installed.

The community edition of Microsoft Visual Studio is free and the Intel oneAPI has been free for the last few years.
- Make sure to install compatible versions of Microsoft Visual Studio and Intel oneAPI packages. Use Google to find the compatible versions. Hyperlinks to the Intel website for the developers' guide never work.
- Install Visual Studio first. The installation procedure is simple, and stick to the default option, but make sure to select the **Desktop development with C++** option during installation.
- Now proceed to Install oneAPI packages; first the Base Toolkit and then the HPC Toolkit. Stick to the default installation procedure. Intel oneAPI packages should be installed in the `C:\Program Files (x86)\Intel\oneAPI` directory.

Once the installation is complete, you can access **Intel oneAPI Command Prompt** from the Program Menu on Windows.



## Compiling and linking LAPACK subroutines from the command line

Save the following Fortran program as `mkl_test.f90` in your working directory. This code is seemingly simple; it calls two subroutines `lapack_test()` and `lapack95_test()` from the main program which solves for a simple 3x3 linear system using the `dgesv` subroutine from `LAPACK` and the `gesv` interface from Intel MKL's `LAPACK95` library. In both cases, it prints the results on the screen.

{% details Click here to see the Fortran code %}
``` fortran
program main

  call lapack_test()

  call lapack95_test()

end program main


subroutine lapack_test()
  
  implicit none

  integer, parameter :: wp = selected_real_kind(15,307)

  integer, parameter :: n = 3
  real(wp) :: A(n,n), b(n), x(n)

  ! required for original lapack
  integer, parameter :: nrhs = 1
  integer :: lda, ipiv(n), ldb, info, i

  ! testing original lapack routine
  do i = 1, n
    A(i,1:i-1) = -1.0d0
    A(i,i+1:n) = -1.0d0
    A(i,i) = 5.0d0
    b(i)   = i
  end do

  x = b
  lda = size(A,1)   ! row count of A
  ldb = size(b)     ! row count of b

  ! A returns as LU factorization, x is the solution
  call dgesv(n, nrhs, A, lda, ipiv, x, ldb, info)
  print *, 'results from original lapack subroutine: '
  print *, x

end subroutine lapack_test

subroutine lapack95_test()
  
  use lapack95, only: gesv

  implicit none

  integer, parameter :: wp = selected_real_kind(15,307)

  integer, parameter :: n = 3
  real(wp) :: A(n,n), b(n), x(n), i

  ! testing lapack95 routines
  ! rebuilding A and b since it was changed
  do i = 1, n
    A(i,1:i-1) = -1.0d0
    A(i,i+1:n) = -1.0d0
    A(i,i) = 5.0d0
    b(i)   = i
  end do

  x = b
  ! A returns as LU factorization, x is the solution
  call gesv(A,x)    
  print *, 'results from lapack95 module: '
  print *,x

end subroutine lapack95_test
```
{% enddetails %}


If you are already familiar with the Microsoft Visual Studio environment, you can create a project there and compile and link the code to Intel MKL libraries to create the executables. You can find other tutorials or blogs to learn how to do that. Here we will use the command line option to compile and link the code to generate the executable. Open the **Intel one API Command Prompt for Intel 64 for Visual Studio 2019** terminal from the Windows program menu. A simple Fortran program (without MKL library or external library) can be compiled and linked using:
``` bash
ifort -o test test.f90
```

Now to compile and link a Fortran code that includes MKL components, we need to add the `/Qmkl` flag before the `-o`. Since the code also uses the LAPACK95 module from Intel MKL, we need to add `mkl_lapack95_ilp64.lib` for further linking.
``` bash
ifort /Qmkl -o mkl_test mkl_test.f90 mkl_lapack95_ilp64.lib
```
`ifort` is the Intel Fortran compiler, the `/Qmkl` option tells the compiler to link standard `.lib` files related to MKL (it's a shortcut), and `-o` specifies the object file. The last option `mkl_lapack95_ilp64.lib` is to link the LAPACK95 interface library to the object file (this is not a part of the core MKL).

This will generate the `mkl_test.exe` executable and `mkl_test.obj` object file in the working directory. You can then run the code from any Windows terminal using:
``` bash
./mkl_test
```

This will now execute and print out results from two different subroutines used in the code. Now LAPACK95 interfaces may appeal to you more than the original LAPACK subroutines because of the simplicity. But there is a caveat, LAPACK95 implementations are not as universal as the original LAPACK. LAPACK95 interfaces included in the Intel MKL and provided by Netlib are different. Your codes may have compatibility issues based on which library and compiler are being used. So it is probably best to use the original LAPACK subroutines and write your own wrappers.

> ##### TIP
>
> If you are using a Linux or Unix-based operating system (macOS), you can follow [this simple tutorial on fortran-lang.org](https://fortran-lang.org/learn/building_programs/managing_libraries/) to learn how to compile source codes and link libraries.
{: .block-tip }



## Using LAPACK libraries with Abaqus user subroutines

If you are looking to use LAPACK with Abaqus user subroutines, I am assuming, you already have configured Abaqus to compile and link user subroutines. If you have not done it yet, [follow the steps described in a previous blog post](https://www.bibekanandadatta.com/blog/2021/link-intel-and-vs-abaqus-2020/). At the end of the post, it is shown how to configure the Abaqus environment file to use the MKL library. 

>
> You need to have **admin** access to make changes described in that blog post. If you are working on a shared computer cluster, ask the **system administrator** to make changes for you.


In case you do not have **admin** access or are unable to make changes for compatibility reasons, you can add a `abaqus_v6.env` file to your home directory or project's working directory with customized compiling options as shown below for Windows platform. Notice,

``` bash
# job-specific abaqus_v6 environment file (Windows)
# this should be located in the working directory

compile_fortran += ['/Qmkl:sequential']
```

For Linux, you need to change the compiler option to `-mkl=sequential` like below.
``` bash
# job-specific abaqus_v6 environment file (Linux)
# this should be located in the working directory

compile_fortran += ['-mkl=sequential']
```

This command will extend the default compiler options provided by Abaqus.


> ##### TIP
>
> If you write an Abaqus user subroutine that uses Intel MKL or other external libraries, it is perhaps better to share the `abaqus_v6.env` file with other users to execute your code.
{: .block-tip }



### Writing and testing Abaqus user subroutine with LAPACK

Now we will include the `lapack_test()` subroutine to the Abaqus `UEL` subroutine interface. For the simple (and dummy) model we built, Abaqus will call the `UEL` subroutine for that. Within the `UEL` subroutine, we call the `lapack_test()` followed by calling the `xit` function from Abaqus to terminate the program. Copy and save the following code as `uel_mkl.for` in your working directory.

{% details Click here to see the Abaqus UEL subroutine code %}
``` fortran 
      SUBROUTINE UEL(RHS,AMATRX,SVARS,ENERGY,NDOFEL,NRHS,NSVARS,
     1 PROPS,NPROPS,COORDS,MCRD,NNODE,U,DU,V,A,JTYPE,TIME,DTIME,
     2 KSTEP,KINC,JELEM,PARAMS,NDLOAD,JDLTYP,ADLMAG,PREDEF,NPREDF,
     3 LFLAGS,MLVARX,DDLMAG,MDLOAD,PNEWDT,JPROPS,NJPROP,PERIOD)

      INCLUDE 'ABA_PARAM.INC'

      DIMENSION RHS(MLVARX,*),AMATRX(NDOFEL,NDOFEL),PROPS(*),
     1 SVARS(*),ENERGY(8),COORDS(MCRD,NNODE),U(NDOFEL),
     2 DU(MLVARX,*),V(NDOFEL),A(NDOFEL),TIME(2),PARAMS(*),
     3 JDLTYP(MDLOAD,*),ADLMAG(MDLOAD,*),DDLMAG(MDLOAD,*),
     4 PREDEF(2,NPREDF,NNODE),LFLAGS(*),JPROPS(*)


        ! user coding to define RHS, AMATRX, SVARS, ENERGY, and PNEWDT
      
        call lapack_test()
        call xit


      RETURN
      END


      subroutine lapack_test()
      
        implicit none

        integer, parameter :: wp = selected_real_kind(15,307)

        integer, parameter :: n = 3
        real(wp) :: A(n,n), b(n), x(n)

        ! required for original lapack
        integer, parameter :: nrhs = 1
        integer :: lda, ipiv(n), ldb, info, i

        ! testing original lapack routine
        do i = 1, n
          A(i,1:i-1) = -1.0d0
          A(i,i+1:n) = -1.0d0
          A(i,i) = 5.0d0
          b(i)   = i
        end do

        x = b
        lda = size(A,1)   ! row count of A
        ldb = size(b)     ! row count of b

        ! A returns as LU factorization, x is the solution
        call dgesv(n, nrhs, A, lda, ipiv, x, ldb, info)
        print *, 'results from original lapack subroutine: '
        print *, x

      end subroutine lapack_test
```
{% enddetails %}


### Executing user subroutine with a dummy Abaqus model

Since we chose to test the LAPACK subroutine inside the Abaqus UEL subroutine, we need to build an Abaqus model with a user element. Remember, our Abaqus subroutine does not do anything but perform a simple linear algebra calculation. So we do not need a real Abaqus model, rather we need something minimalistic that can help us to execute the subroutine. Following is a simple example of a user element, where I defined a single 4-node quadrilateral element with one property and static step. We did not apply any boundary or loading condition to this. Copy the following input file and save it as `test.inp` in the working directory.

{% details Click here to see the Abaqus input file %}
```
*User element, type=U1, nodes=4, coordinates=2, properties=1, iproperties=0
1, 2
*Node
1, 0., 0.
2, 1., 0.
3, 0., 1.
4, 1., 1.
*Element, type=U1, elset=all
1, 1, 2, 4, 3
*Uel property, elset=all
1.0
*Step, name=test
*Static
1., 1., 1.E-3, 1.
*End step
```
{% enddetails %}


Abaqus UEL subroutine needs to be executed from the command line. From the **PowerShell** terminal, **Windows Command Prompt**, or **Abaqus Command** terminal, you can execute the subroutine with the sample input file using the following command:
``` bash
abaqus interactive job=test user=uel_mkl.for
```

This will compile and link the user subroutine to the Abaqus executable and run it from the command line. As we already discussed, this is not a real Abaqus model or user subroutine code, we are not hoping to get any real finite element result. Executing this subroutine will just print out the same output as before on the terminal before exiting Abaqus with an error (remember, we used `xit` function for termination).

## LAPACK resources

Using LAPACK is not as intuitive or straightforward as using MATLAB or NumPy. Look into the documentation carefully and you can use the following to find what LAPACK subroutines to use and how to use them for specific tasks.

- [Intel oneAPI LAPACK Function Finding Advisor](https://www.intel.com/content/www/us/en/developer/tools/oneapi/onemkl-function-finding-advisor.html#gs.8k31mo)
- [LAPACK Examples by NAG library](https://github.com/numericalalgorithmsgroup/LAPACK_examples)


## Bonus item: Adding Intel oneAPI Command Prompt to PowerShell and Windows Terminal

You can not run the Intel oneAPI command line from the PowerShell terminal directly, but you can run it from the original Windows Command Prompt terminal. However, Command Prompt, even the default old PowerShell (PowerShell 5) does not have the latest features such as auto-completion. If you have not used the new PowerShell, it's highly recommended on Windows. Download and install the [latest version of PowerShell 7 from here](https://github.com/PowerShell/PowerShell). The new PowerShell executable is named as `pwsh.exe`.

[Windows Terminal App](https://learn.microsoft.com/en-us/windows/terminal/) is great at managing different Shell profiles. Terminal does not have to be boring; you can customize your terminal experience a lot using this app. Download and install the [Windows Terminal app from here](https://github.com/microsoft/terminal). Once installed, you can manage different Shell profiles there (Cmd, PowerShell, pwsh, WSL, etc.). To add the Intel oneAPI profile there,

- Open the app, and from the dropdown menu on the top bar, click on **Settings**.

- Now scroll down along the left-side menu bar, click **Add a new Profile**, and select the option **Empty New Profil**. Name the new profile as **Intel oneAPI** and add the following following to the **Command Line** option.
  ``` bash
  cmd.exe /k ""C:\Program Files (x86)\Intel\oneAPI\setvars.bat" intel64 vs2019" && pwsh
  ```
  Other options to set the new profile are trivial. Now you can save the profile. When you start the profile from the Terminal app, this will execute the Windows command prompt first, then it will set the environment variables for Intel oneAPI and Visual Studio, and then run the new PowerShell 7 (`pwsh.exe`). You can now compile and build programs using Intel Fortran in a modern terminal environment.