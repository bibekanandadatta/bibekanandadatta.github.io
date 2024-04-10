---
layout: post
title: Linking Intel oneAPI and Visual Studio with Abaqus
date: 2021-02-13 11:59:00-0400
description: A tutorial to link Fortran with Abaqus
tags: Abaqus Fortran programming finite-element Visual-Studio Intel-oneAPI user-subroutine
categories: tutorial
giscus_comments: false
related_posts: true
toc: 
  beginning: true
---

ABAQUS is one of the most popular commercial finite element programs in both academia and industry. In addition to the built-in physics and material models, ABAQUS allows its users to program new features through the user subroutine feature, typically written in Fortran or C++. The recommended compiler for ABAQUS user subroutines is Intel Fortran which is a part of the Intel oneAPI package since 2021. Additionally, on Windows OS, it requires installing Microsoft Visual Studio for linking and compilation. It always has been quite confusing to the users how to configure ABAQUS to use this feature. In this blog post, I will describe the procedures for installing necessary package and configuring ABAQUS to avail the user subroutine feature. 

## Software installation

### Things you will need

1. ABAQUS finite element solver
2. Microsoft Visual Studio Community Edition
3. Intel oneAPI Base Toolkit and Intel oneAPI HPC Toolkit
4. Notepad or Notepad++ (or some text editor)

For my first time, I used ABAQUS 2020, Microsoft Visual Studio 2019, Intel oneAPI 2021 followed by different versions of these packages in past few years. But I will use examples and images from the first installation I did back in February 2021. If you are doing it right now, you may have different versions of these packages, however the procedure is almost identical. A few things to note before you begin installation:

1. Please ensure you have the compatible version of Visual Studio and Intel oneAPI.
2. Install Visual Studio and Intel oneAPI in that order. ABAQUS can be installed at any point during the process (before or after).
3. You will need to save the batch files related to these software using administrator access. So launch Notepad from Windows program menu as administrator or use Notepad++. I used Notepad++.


### ABAQUS Installation

ABAQUS user-defined features are only available through its academic (research) or commercial license which you should obtain from your university or workplace. ABAQUS Installation process is trivial depending on how you obtain the executables. While you going through the installation process, make sure to install ABAQUS with CAA API components. ABAQUS should be installed in `C:\SIMULIA` directory.


### Microsoft Visual Studio

Intel oneAPI 2021 supports Visual Studio 2017 and Visual Studio 2019. Please search on Google to download Visual Studio Community Edition. I used the latest one, VS 2019, at that time. **If you are downloading a newer version, please make sure it is compatible with Intel oneAPI.** 

Download the installer and click on it to start the installation process. On the installation page, please select `Desktop development with C++` and keep the rest of the default. It will take a few minutes to install.



### Intel oneAPI

Intel oneAPI is split into two packages; Intel oneAPI Base Toolkit and Intel oneAPI HPC Toolkit. To be able to use Intel Fortran and C++ compilers, you will need both toolkits (specially for MKL libraries and parallel processing). 

1. Once both of the packages are downloaded, first install the Intel oneAPI Base Toolkit (I chose default installation). It will recognize the existence of Visual Studio. Please click on the appropriate version and move forward with installation. It might take about 15-20 minutes to install the Base Toolkit. The next step is to install the HPC tool kit in the same procedure. This will take 5-10 minutes to get installed.

2. If you have done default installation of Intel oneAPI, its components should be located in `C:\Program Files (x86)\Intel\oneAPI`. Navigate to this directory and then navigate further to ensure `ifort` executable is available in the following (or similar) directory. Copy the directory in a text file for later use.

    ``` 
    C:\Program Files (x86)\Intel\oneAPI\compiler\2021.1.1\windows\bin\intel64
    ```

## Linking Intel Fortran with Abaqus

While the installation of the software packages are cakewalk, it gets complicated at this stage. Follow one of the two methods below. I prefer **Method 1** because its easier and I was able to configure Intel oneMKL library as well in this approach.

### Method 1: Editing ABAQUS batch file

1. Navigate to `C:\Program Files (x86)\Intel\oneAPI\compiler\2021.1.1\env` (or similar directory) and ensure `vars.bat` file is available. If the directory name is different in your installation, search for `vars.bat` file through Windows Explorer and navigate to the directory. Copy the file directory to a text file.

    ``` 
    C:\Program Files (x86)\Intel\oneAPI\compiler\2021.1.1\env\vars.bat
    ```

2. Navigate to `C:\SIMULIA\Commands` directory and open the `abq2020.bat` (or the version you installed) file with any text editor. In the beginning of the file, add the following lines to the file and **save it as administrator**.

    ``` 
    SET PATH=%PATH%; C:\Program Files (x86)\Intel\oneAPI\compiler\2021.1.1\windows\bin\intel64; call "C:\Program Files (x86)\Intel\oneAPI\compiler\2021.1.1\env\vars.bat" intel64
    ```

As you can see, the `PATH` in the first line is the `PATH` for ifort compiler executables which was copied during installation. The second line is calling the batch file for ifort compiler which sets the environment variales when ABAQUS is invoked.


### Method 2: GUI Approach

1. Navigate to C:\Program Files (x86)\Intel\oneAPI\compiler\2021.1.1\env (or similar directory) and ensure vars.bat file is available. If the directory name is different in your installation, search for `vars.bat` file through Windows Explorer and navigate to the directory. Copy the file directory to a text file.

    ```
    C:\Program Files (x86)\Intel\oneAPI\compiler\2021.1.1\env\vars.bat
    ```

2. Copy the path locations with file names for both of them. From the Windows start menu, search **Edit the system environment variables**. Then click on **Environment Variables > Path** (under system variables) and New. Paste those previously copied batch file paths and save them. You can see I already added those (2nd and 3rd from the bottom on the rightmost image).

3. The next step is to link the Intel Fortran compiler with Abaqus shown step-by-step in the following image. Search Abaqus Command from the Windows Menu. Click on Open File Location. Then in Windows Explorer right click on Abaqus Command and click on Properties. In the shortcut tab, locate the target. Change from `C:\WINDOWS\system32\cmd.exe /k` to the following

    ```
    "C:\Program Files (x86)\Intel\oneAPI\compiler\2021.1.1\env\vars.bat" intel64 vs2019 & C:\WINDOWS\system32\cmd.exe /k
    ```

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="/assets/img/abq_sys_path.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="/assets/img/abq_link.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>




## Verify Linking and Installation

Open Abaqus Command (you can also do it from cmd or Powershell terminal) from the Windows menu and type `abaqus info=system`. It should show system information as follows, including the new compiler and linker information.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="/assets/img/abq_sys.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

To verify further, type `abaqus verify -user_std` and/ or `abaqus verify -user_exp` on the Abaqus Command window. If the installation and linking were successful, then you will see something like this:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="/assets/img/abq_user_std.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>


`abaqus verify -user_std` is for verifying user subroutine feature for Abaqus/ Standard (UMAT, UEL, etc.) and `abaqus verify -user_exp` is for verifying user subroutine feature for Abaqus/ Explicit (VUMAT, VUEL, etc.). To verify all of the features of Abaqus you can type `abaqus verify -all`. Please note, this might take a while and some components might not pass depending on your license and installation.



### Bug fix for Abaqus 2019/2020

If you still have issues compiling user subroutines, one of the possible bug could be fixed by following approach. I experienced this issue when I first installed these packages; it may have been fixed by now. But at that time. SIMULIA suggested making new Intel oneAPI Toolkits compatible with Abaqus. Please navigate to `C:\Program Files\Dassault Systemes\SimulationServices\V6R2020x\win_b64\SMA\site` directory and locate `abaqus_v6.env` file. Open the file using a text editor, add the following line at the end of the file, and **save the file as administrator**.

```
compile_fortran += ['/names:lowercase',]
```

## Linking Intel oneMKL Library with ABAQUS

This is an advanced and optional feature which is very rarely used. Intel oneMKL library includes efficient math subroutines (e.g., BLAS and LAPACK) which can be included in user subroutine to perform computation. However, from time to time, I found it useful to take advantages of existing libraries to write efficient subroutines.


1. Navigate to `C:\Program Files (x86)\Intel\oneAPI\mkl\2021.1.1\env`, ensure the `vars.bat` file is available for oneMKL package. This batch file is for Intel oneMKL library. Copy the file directory as usual. 


2. Navigate to `C:\SIMULIA\Commands` and open the abq2020.bat (or corresponding version) file. Add the following line below the two lines previously added for ifort compiler. Save the file as administrator using the same approach as above; either run Notepad as administrator from Windows program menu or use Notepad++.

    ```
    call "C:\Program Files (x86)\Intel\oneAPI\mkl\2021.1.1\env\vars.bat" intel64
    ```

3. In the previous step, providing Intel oneMKL batch file location in the ABAQUS batch file will source the environment variables when ABAQUS is called to run simulations. However, to compile code with Intel oneMKL library, you will need to do one more step. Locate to `C:\SIMULIA\EstProducts\2020\win_b64\SMA\site` directory and open **win86_64.env** file. Find the following line in the file and add the `/Qmkl:sequential` compiler option as shown below. Rest of the file should be same. Save the file as administrator.

    ```
    compile_fortran=['ifort','/Qmkl:sequential', ... ...]
    ```

    You may be able to run oneMKL library in parallel with ABAQUS user surboutines by adding `/Qmkl:parallel` compiler flag. However, I never tested this feature.