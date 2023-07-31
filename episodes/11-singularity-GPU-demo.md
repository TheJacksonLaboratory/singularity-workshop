---
title: "11-singularity-GPU-demo"
teaching: 30
exercises: 0
---

### Demo and considerations for GPU

To run utilize a GPU the following are needed:
- GPU drivers installed on the host system.
- The nvidia-smi program in both the container and the host system.
- Add the ```--nv``` flag during within the singularity execute command.

### Example of GPU

Prep data and files.

```bash
cd /projects/my-lab/11-gpu-demo
```

Manually downloaded the images.zip file from google drive link found in URL below. 
Manually tranfered the file to ./projects/my-lab/11-gpu-demo. directory.
https://github.com/AlexOlsen/DeepWeeds

```bash
unzip images.zip
```

Verify the nvidia-smi system is on the host system. 

```bash
nano nvidia-smi
```

```bash
singularity pull <>
```

```bash
singularity exec --nv <> nvidia-smi
```

