# Julia Data Science Handbook
This repo contains a collections of notebooks which demonstrate how to carry out core aspects of data science using Julia.

# Why learn Julia?
As I see it, there are 3 advantages that Julia has over other programming languages:
1. Julia is easy to write *and* fast to run, avoiding the [2-language problem](https://thebottomline.as.ucsb.edu/2018/10/julia-a-solution-to-the-two-language-programming-problem) for computationally-intensive analyses
2. Julia relies less on algorithms written in C and Fortran, so debugging or customisation of lower-level dependencies can all be done in the same language
3. Julia allows intuitive specification of custom probabilistic models, with many pre-defined sampling algorithms to run inference using the model e.g. [Bayesian Neural Networks](https://juliacomputing.com/case-studies/astra-zeneca/) without using JAGS or STAN

# Installation
1. Go to the [Downloads](https://julialang.org/downloads/) page on the Julia language website, and download the installer for the Current Stable release for your operating system
    * Windows = `installer`
    * Mac = `.dmg`
2. Install Julia onto your computer by following the usual installation procedure for your operating system
    * Windows = run `.exe` 
    * Mace = drag the `.dmg` file into the `Applications` folder
3. Start the Julia REPL (read-evaluate-print loop) by running the `Julia` program that has just been installed
4. Verify that the installation has been successful by running the command `1+2` (this should produce the output `3`)
5. Close the Julia REPL
6. Open a fresh instance of the Julia REPL and enter pkg mode by running the command `]`
7. Install the IJulia package by running the command `add IJulia`
8. Exit pkg mode by pressing `CTRL+C`
9. Create a Julia-specific installation of Jupyter by running the command `using IJulia` followed by the command `notebook()`
    * at the prompt `Install Jupyter using conda?` enter `y`
    * this uses conda to create a specific installation of Jupyter, separate from any other installations of Jupyter and Python that you may already have on your computer
    * for full docs on installing Jupyter through Julia, see [here](https://github.com/JuliaLang/IJulia.jl#quick-start)

# Running Julia
## Running Julia Interactively
There are 3 ways to run Julia interactively:
1. The Julia REPL (read-evaluate-print loop), which is similar to running Python or R in interactive mode through the command line. You can launch the Julia REPL by clicking on the `Julia` app from your operating system menu.
2. Jupyter notebooks, which allow interactive coding. This assumes that Jupyter is installed, see above for details. To launch a Jupyter notebook through Julia, follow these steps:
    * launch the Julia REPL
    * enter the command `using IJulia`
    * enter the command `notebook()`
3. Through VS Code, which allows you to run code line-by-line or as an entire file. VS Code also features a plot window to view output, and tools for software development processes such as unit testing and version control. A summary of the VS Code features for Julia can be found [here](https://www.julia-vscode.org/), and details of how to configure VS Code to run Julia can be found [here](https://code.visualstudio.com/docs/languages/julia).

## Running Julia Scripts
If you have your Julia script saved as a `.jl` file and you want to run it from start to end, you can call Julia from the command line, just as you would run `python myscript.py`. However, to allow your system to recognise Julia as a program that can be run on files, you need to add a symlink to the Julia binary. The exact method differs between operating systems (see below).
### MacOS
1. Download the `.dmg` file for Julia 
2. Install it by dragging the `.dmg` file into your `Applications` folder
3. Open the Terminal
4. Check that your Julia installation was successful by typing:
```
du /Applications/Julia-1.7.app/Contents/Resources/julia/bin/julia
```
This should show a number (the size of the Julia binary on disk), followed by the path to the Julia binary. If this returns an error, you may have to change the path to the Julia binary to a different version of Julia e.g. `/Applications/Julia-1.6.app/Contents/Resources/julia/bin/julia`. If this still doesn't work, open the Launchpad and check that the Julia app is in there. If it isn't repeat MacOS installation steps 1-4 above.

5. Once you have verified that you have the correct path to the Julia binary, you can add a symlink to it in your `/usr/local/bin` directory:
    ```
    sudo ln -s /Applications/Julia-1.7.app/Contents/Resources/julia/bin/julia /usr/local/bin/julia
    ```
6. Close the Terminal window and open a new Terminal window.
7. Verify that the julia symlink has been created successfully with the command:
    ```
    julia --version
    ```
This should return the version of Julia that you downloaded in step 1.

# Julia Environments

## Installing `RCall`
The `RCall` package allows us to issue R commands directly from Julia, and work with the results as Julia objects. However, it is finicky to install, as it expects there to be a directory called `R_HOME` containing an R install in the directory you are operating in, and this very rarely exists unless you are running Julia from within a base system directory (which generally isn't a good idea). To get around this, we can create a Julia-specific installation of R using the following commands:
```
ENV["R_HOME"] = "*"
using Pkg
Pkg.build("RCall")
```
This will set up a fresh (separate) installation of R on your computer which is exclusively used by Julia, and doesn't interfere with any existing installations of R.
