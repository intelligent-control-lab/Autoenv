# Autoenv

- This environment is transformed from [ngsim_env](https://github.com/sisl/ngsim_env) and written in python.

- This environment is an extensible environment that looks like rllab, we encourage any exploration on our environment, including but not limited to 
data source, feature extraction, action propagation, reward definition.

- Added AGen algorithm

## Installation:

### installation instructions for AutoEnv
```bash
cd ~
git clone https://github.com/JackFram/Autoenv.git
cd ~/Autoenv
conda env create -f environment.yml
conda env update
# If hdf5 is not installed, install it as it is required by AutoEnvs later in the process
conda install hdf5
conda install tensorflow
mkdir data/trajectories
mkdir data/experiments
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

```bash
conda activate rllab3
git clone https://github.com/sisl/ngsim_env.git
cd ngsim_env

# enter a julia interpreter and install dependencies.
#   NOTE: I got some weird error with one of the packages, I think it was AutoViz, where there was a line ending before expected or something like that.
#   I just repeated the add instruction and it seemed to work fine.
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
pip install julia
# make sure it works:
python
  import julia
  julia.Julia()
  # if this not working try
  from julia.api import Julia
  j = Julia(compiled_modules=False)
  # we just want to make sure it doesnt error
  exit()
```
add several functions to NGSIM.jl
```bash
vim ~/.julia/packages/NGSIM/OPF1X/src/NGSIM.jl
```
add 

```
write_roadways_to_dxf,
write_roadways_from_dxf,
sparse
```
below `convert_convert_raw_ngsim_to_trajdatas` under `export`

## Automated running ( data preprocessing included )
- First you need to put all your data under `Autoenv/preprocessing/data/`
your data should be the raw csv files categorized by time stamp directory
~/Autoenv/preprocessing/data
- You might need to do some file path adjustment
- Finally run one step code in project root directory ~/Autoenv
```bash
python adaption.py --n_proc 1 --params_filename itr_200.npz --use_multiagent True --n_envs 1 --adapt_steps 1
```






