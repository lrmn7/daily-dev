**Last updated**: June 1, 2021

# Blender rendering using SLURM

## SLURM submission python script

 * Distributes frames of render animation to multiple nodes.
 * Rendering `donut.blend` from frame `1` to `300`.
 * Each node rendering `60` frames.
 * Blender bin path: `BLENDER_PATH=/store/empa/em13/apps/blender`.

Contents of `submit.py`:
```python
#!/usr/bin/env python3
import os

# Constants
job_name = "blender"
time = "00:30:00"
account = "em13"
partition = "normal"
background = "donut.blend"
frame_start = 1
frame_end = 300
nframes_per_node = 60

HEADER=\
f"""#!/bin/bash -l
#SBATCH --time={time}
#SBATCH --nodes=1
#SBATCH --ntasks-per-core=2
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=24
#SBATCH --constraint=gpu
#SBATCH --hint=multithread
#SBATCH --account={account}
#SBATCH --partition={partition}

module load daint-gpu
module load cudatoolkit

# NCCL FLAGS
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
export PYTHONFAULTHANDLER=1

BLENDER_PATH=/store/empa/em13/apps/blender
BACKGROUND={background}
"""

# Batch split
for batch_start, batch_end in zip(
        range(frame_start, frame_end, nframes_per_node),
        range(frame_start + nframes_per_node - 1, frame_end + nframes_per_node - 1, nframes_per_node)
        ):
    # Write submission script
    with open("batch.job", "w") as f:
        f.writelines(HEADER)
        f.write("srun $BLENDER_PATH/blender \\\n")
        f.write("\t--python $BLENDER_PATH/enable_cuda.py \\\n")
        f.write("\t--background $BACKGROUND \\\n")
        f.write("\t--render-output //render/frame_###.png \\\n")
        f.write(f"\t--frame-start {batch_start} \\\n")
        f.write(f"\t--frame-end {batch_end} \\\n")
        f.write("\t--render-anim\n")

    # Submit job
    os.system(f"sbatch --job-name={job_name}-s{batch_start:03d}-e{batch_end:03d} batch.job")

# Cleanup
os.remove("batch.job")
```

## Ensuring cuda is enables on the nodes

Contents of `enable_cuda.py`:
```python
import bpy

def enable_gpus(device_type, use_cpus=False):
    preferences = bpy.context.preferences
    cycles_preferences = preferences.addons["cycles"].preferences
    cycles_preferences.refresh_devices()
    devices = cycles_preferences.devices

    if not devices:
        raise RuntimeError("Unsupported device type")

    activated_gpus = []
    for device in devices:
        if device.type == "CPU":
            device.use = use_cpus
        else:
            device.use = True
            activated_gpus.append(device.name)
            print('activated gpu', device.name)

    cycles_preferences.compute_device_type = device_type
    bpy.context.scene.cycles.device = "GPU"

    return activated_gpus

enable_gpus("CUDA")
```
