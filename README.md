# Menu

[Basic workflow for data analysis & visualization](./basic_workflow.md)

# My environment

## OS
macOS Big Sur

## Main programming language
Python

### Package manager
`conda` (via [miniconda](https://docs.conda.io/en/latest/miniconda.html))

### Frequently used packages
- NumPy
- SciPy
- matplotlib

## Editor
Visual Studio Code

## Other softwares
- Adobe Illustrator CC
- Keynote
- JEOL Delta (NMR analysis)
- ChemDraw
- Microsoft Word
- Microsoft Excel
- Microsoft OneNote
- Google Chrome

# How I build my environment

## 1. (mac only) Install `homebrew`
https://brew.sh/

## 2. Install miniconda
With `homebrew`: https://formulae.brew.sh/cask/miniconda

Or find an installer from: https://docs.conda.io/en/latest/miniconda.html

## 3. Create virtual environment
In mac, open the terminal by e.g. searching for 'terminal' in Spotlight. 

In Windows, `conda` can only be used in Anaconda Prompt. 
It can be found somewhere in the Start menu. 

In the terminal (or Anaconda Prompt in Windows), 
```
$ conda create --name myenv python=3.10
```
This creates a virtual environment. `myenv` can be anything. 
In my case, I do something like `labenv3.10` to show that
this environment is used in the lab and the Python interpreter version
is 3.10. 
Of course `python=3.10` can be changed to your choice. 

To enter that environment, 
```
$ conda activate myenv
```
You can now see `(myenv)` instead of `(base)` (default environment)
at the beginning of each line in your terminal. 

## 4. Install Visual Studio Code (vscode)
With `homebrew`: https://formulae.brew.sh/cask/visual-studio-code

Or install from the installer: https://code.visualstudio.com/

## 5. vscode basic settings & usage
- Install Python extension
- In Settings, search for `python execute in file dir` 
  and turn on `Python › Terminal: Execute In File Dir` check box. 
  With this, Python will run at the directory of the current script. 
- Add folder(s) to your workspace. 
- Set the Python interpreter to use. 
  In the Command Palette (see https://code.visualstudio.com/docs/getstarted/tips-and-tricks#_command-palette ), 
  type `Select interpreter`. Select the folder you want to deal with, 
  and select the interpreter. Usually you can see the virtual environment (e.g. `myenv` in Section 3)
  along with others including default installation of Python in some platforms. 
  
## 6. Install packages
After `conda activate`-ing your environment, 
```
$ conda install numpy
```
will install NumPy package to the environment. 
Of course it works for SciPy and matplotlib as well. 
```
$ conda install scipy
$ conda install matplotlib
```
