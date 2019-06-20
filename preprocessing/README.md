## Preprocessing holo data guideline:
```bash
mkdir data
mkdir final_data
mkdir processed_data

python clean_holo.py --traj_path "your raw trajectory data" --lane_path "your lane data if applicable"

cd ~/.julia/packages/NGSIM/OX1F/data  # OX1F maybe replaced by other version control number
cp ~/AutoEnv/preprocessing/centerlinesHOLO.txt ./
cp ~/AutoEnv/preprocessing/boundariesHOLO.txt ./
cp ~/AutoEnv/preprocessing/final_data/holo_trajectories.txt ./

# use julia to do lane conversion
julia
>> using NGSIM
>> NGSIM.write_roadways_to_dxf()
>> NGSIM.write_roadways_from_dxf()
>> exit()

# Notice: replace const NGSIM_TRAJDATA_PATHS in ~/.julia/packages/NGSIM/OX1F/src/ngsim_trajdata.jl with the right one(holo_trajectories.txt)

julia
>> using NGSIM
>> convert_raw_ngsim_to_trajdatas()
```

Notice: in ~/.julia/packages/NGSIM/OX1F/src/trajdata.jl change file path accordingly
```bash
cd ~/ngsim_env/scripts

julia extract_ngsim_demonstrations.jl

cd ~/ngsim_env/scripts/imitation

# change the file path in each python file accordingly

# if you have ids file and starts file in ~/.julia/packages/NGSIM/OX1F/data remove the ids and starts files.

python adaption.py --n_proc 1 --exp_dir ../../data/experiments/multiagent_curr/ --params_filename itr_200.npz --use_multiagent False --n_envs 1 --adapt_steps 1
# if occurs segmentation fault, use ctrl+c -.-

python psgail_holo.py --n_proc 1 --exp_dir ../../data/experiments/multiagent_curr/ --params_filename itr_200.npz --use_multiagent False --n_envs 1 --adapt_steps 1

python get_gt.py --n_proc 1 --exp_dir ../../data/experiments/multiagent_curr/ --params_filename itr_200.npz --use_multiagent False --n_envs 1 --adapt_steps 1
```

then open vis_pred_trajs.ipynb from jupiter notebook and run the code in it.