# NGSIM.jl

A Julia package for working with the Next Generation Simulation dataset (NGSIM).
Was tested on the [Highway 101](http://www.fhwa.dot.gov/publications/research/operations/07030/) and [I-80](http://www.fhwa.dot.gov/publications/research/operations/06137/) datasets.

[![Build Status](https://travis-ci.org/sisl/NGSIM.jl.svg?branch=master)](https://travis-ci.org/sisl/NGSIM.jl)
[![Coverage Status](https://coveralls.io/repos/sisl/NGSIM.jl/badge.svg?branch=master&service=github)](https://coveralls.io/github/sisl/NGSIM.jl?branch=master)

This package is fully compatible with [AutomotiveDrivingModels.jl](https://github.com/sisl/AutomotiveDrivingModels.jl), providing the `Roadway` and `Trajdata` types from the NGSIM data. Roadway geometry was extracted from the NGSIM CAD files. The vehicle trajectories were filtered to provide better global positions and orientation.

The NGSIM trajectory data is available in [our first release, with instructions here](https://github.com/tawheeler/NGSIM.jl/releases).

## Git It

You just clone it! Note that you also have to clone my [Vec](https://github.com/tawheeler/Vec.jl) package.

```julia
using Pkg
Pkg.add(PackageSpec(url="https://github.com/sisl/Vec.jl", rev="72ab872cf4c76c3fcd299d77cdf497aa54996b9f"))
Pkg.add(url="https://github.com/sisl/Records.jl", rev="593654f2acae156fefea393d0db3a89f5a8a8bf0")
Pkg.add(PackageSpec(url="https://github.com/sisl/AutomotiveDrivingModels.jl", rev="9e1878fc88fe114434bca564e792461242e537d5"))
Pkg.add(PackageSpec(url="https://github.com/sisl/AutoViz.jl", rev="00bf0cb477a5f27930339fa0e6c7b140dc9a6794"))
Pkg.add(PackageSpec(name="DataFrames", version="0.16.0"))
Old installation directions:
# Pkg.add(PackageSpec(url="https://github.com/sisl/AutomotiveDrivingModels.jl"))
# Pkg.add(PackageSpec(url="https://github.com/sisl/AutoViz.jl"))
# Pkg.add(PackageSpec(url="https://github.com/JuliaData/DataFrames.jl", rev="e73182f6b0b303721c31706d8771dd0472f316e8"))
# Pkg.add(PackageSpec(url="https://github.com/sisl/NGSIM.jl"))
```
The data must also be downloaded as described above or below.

# Quickstart

To download the datasets, register for an [account](https://www.its-rde.net/index.php/about/register), navigate to the datasets [page](https://www.its-rde.net/index.php/rdedataenvironment/10023), select the links for the I-80 and US-101 datasets, and download. Alternatively, download the data associated with the first release as mentioned above.

To extract trajectory data (Trajdata) from the raw NGSIM data, place the raw data files in the NGSIM.jl/data directory, and run

```julia
using NGSIM
convert_raw_ngsim_to_trajdatas()
```

The resulting files can then be loaded into a Julia program as Trajdata, a type defined in [AutomotiveDrivingModels.jl](https://github.com/sisl/AutomotiveDrivingModels.jl). See jnotebooks/Demo.ipynb for example usage.
