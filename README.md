# Autoenv
|`Document`  |`Version`  |
| :---:      | :---:     |
| [<img src="https://img.shields.io/badge/docs-latest-blue">](https://jackfram.github.io/AutoEnvDoc/docs/build/html/index.html) | [<img src="https://img.shields.io/badge/version-0.1-brightgreen">]() |
- This environment is transformed from [ngsim_env](https://github.com/sisl/ngsim_env) and written in python.

- This environment is an extensible environment that looks like rllab, we encourage any exploration on our environment, including but not limited to 
data source, feature extraction, action propagation, reward definition.

- Added AGen algorithm

## Installation:

### installation instructions for AutoEnv
```bash
# install miniconda
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh # Linux
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh # Mac
# answer yes to everything
sh ./Miniconda3-latest-Linux-x86_64.sh # Linux
sh ./Miniconda3-latest-MacOSX-x86_64.sh # Mac
# remove sh files
rm Miniconda3-latest-Linux-x86_64.sh # Linux
rm Miniconda3-latest-MacOSX-x86_64.sh  # Mac

source ~/.bashrc 
# if no bashrc file try ~/.bash_profile instead
conda install conda==4.6.14
cd ~
git clone https://github.com/JackFram/Autoenv.git
cd ~/Autoenv
conda env create -f environment.yml
conda env update
conda activate rllab3
# If hdf5 is not installed, install it as it is required by AutoEnvs later in the process
conda install hdf5
conda install tensorflow
cd ~/Autoenv
mkdir preprocessing/data
mkdir preprocessing/final_data
mkdir preprocessing/processed_data
mkdir preprocessing/lane
```

### julia:
Install julia 1.1. Code snippet assuming you are installing julia to home directory. If not, please modify the path in bashrc step accordingly.
```bash
cd ~
wget https://julialang-s3.julialang.org/bin/linux/x64/1.1/julia-1.1.0-linux-x86_64.tar.gz
tar -xvf julia-1.1.0-linux-x86_64.tar.gz
rm julia-1.1.0-linux-x86_64.tar.gz
echo "export PATH=$(pwd)/julia-1.1.0/bin:$PATH" >> ~/.bashrc
```
Make sure the `julia` command pops up a julia 1.1 interpreter.

### Install ngsim_env
If have problems for installing NGSIM package please check out this doc [NGSIM](https://github.com/intelligent-control-lab/Autoenv/blob/master/doc/NGSIM.md) contributed by Simin Liu

```bash
conda activate rllab3
git clone https://github.com/sisl/ngsim_env.git
cd ngsim_env

# enter a julia interpreter and install dependencies.
#   NOTE: I got some weird error with one of the packages, I think it was AutoViz
#   I just ignore the error and it seemed to work fine.
julia
  # Add dependencesjulia
  using Pkg
  Pkg.add(PackageSpec(url="https://github.com/sisl/Vec.jl"))
  Pkg.add(PackageSpec(url="https://github.com/sisl/Records.jl"))
  Pkg.add(PackageSpec(url="https://github.com/sisl/AutomotiveDrivingModels.jl"))
  Pkg.add(PackageSpec(url="https://github.com/sisl/AutoViz.jl"))
  Pkg.add(PackageSpec(url="https://github.com/sisl/AutoRisk.jl.git", rev="v0.7fixes"))
  Pkg.add(PackageSpec(url="https://github.com/sisl/NGSIM.jl.git"))
  Pkg.add(PackageSpec(url="https://github.com/sisl/BayesNets.jl.git"))

  # Add the local AutoEnvs module to our julia environment
  ] dev ~/ngsim_env/julia
 
  # make sure it works
  using NGSIM

cd ~/ngsim_env/julia/  
julia deps/build.jl

# make sure they work, by entering interpreter

julia
  using PyCall
  using PyPlot
  using HDF5
  exit()
```
add python interface
```bash
conda activate rllab3
cd ~/ngsim_env/python
python setup.py develop
pip install julia==0.2.0
# make sure it works:
python
  import julia
  julia.Julia()
  # if this not working try the commands pop up in the error information
  # or try
  from julia.api import Julia
  j = Julia(compiled_modules=False)
  # we just want to make sure it doesnt error
  exit()
```
replace package with our modified version unzip our modified packages and put directory under
`~/.julia/packages/`

```bash
cd ~/Autoenv/
rm -rf ~/.julia/packages/AutomotiveDrivingModels
rm -rf ~/.julia/packages/NGSIM
mv NGSIM ~/.julia/packages/
mv AutomotiveDrivingModels ~/.julia/packages/
julia 
    using AutomotiveDrivingModels
    using NGSIM
```

## Automated running ( data preprocessing included )
- First you need to put all your data under `Autoenv/preprocessing/data/`
your data should be the raw csv files categorized by time stamp directory
~/Autoenv/preprocessing/data
- You might need to do some file path adjustment
- Finally run one step code in project root directory ~/Autoenv
```bash
python adaption.py --n_proc 1 --params_filename itr_200.npz --use_multiagent True --n_envs 1 --adapt_steps 1
```






