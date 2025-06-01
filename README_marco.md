# afm_group_2
## server login (set up cursor/vscode remote-ssh)
login (after copying ssh key to server with ssh-copy-id -i ~/.ssh/id_ed25519.pub -o Port=58022 s0125@atcremers45.in.tum.de)
```
ssh -p 58022 s0125@atcremers45.in.tum.de
```

switch node (with pw)
```
ssh s0125@atcremers46.in.tum.de
```

Copy stuff to server (example)
```
rsync -avz -e "ssh -p 58022" /Users/marcolorenz/Library/CloudStorage/OneDrive-Personal/*.MOV s0125@atcremers45.in.tum.de:~/
```

## useful commands

### GPU
```
nvidia-smi #Overview
```

### CPU usage
```
htop
```

### disk (free) space
```
df -f
```

### Standard debugging session
```
salloc --nodes=1 --cpus-per-task=4 --mem=32G --gres=gpu:1,VRAM:24G --time=0-12:00:00 --mail-type=NONE --part=PRACT --qos=practical_course
```

# DUSt3R

## env

### uv
To install using the recommended script, run:
```
curl -LsSf https://astral.sh/uv/install.sh | sh
source $HOME/.local/bin/env
```

### dependencies - uv
```
git clone --recursive https://github.com/naver/dust3r
uv venv .dust3r_venv --python 3.11
source .dust3r_venv/bin/activate
uv pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121
cd dust3r
uv pip install -r requirements.txt
# DUST3R relies on RoPE positional embeddings for which you can compile some cuda kernels for faster runtime.
cd croco/models/curope/
python setup.py build_ext --inplace
cd ../../../
```

### dependencies - conda
```
conda create -n dust3r python=3.11 cmake=3.14.0
conda activate dust3r 
conda install pytorch torchvision pytorch-cuda=12.1 -c pytorch -c nvidia  # use the correct version of cuda for your system
pip install -r requirements.txt
# Optional: you can also install additional packages to:
# - add support for HEIC images
# - add pyrender, used to render depthmap in some datasets preprocessing
# - add required packages for visloc.py
pip install -r requirements_optional.txt
```
```
# DUST3R relies on RoPE positional embeddings for which you can compile some cuda kernels for faster runtime.
cd croco/models/curope/
python setup.py build_ext --inplace
cd ../../../
```

### demo
```
python3 demo.py --model_name DUSt3R_ViTLarge_BaseDecoder_224_linear
```

# MASt3R SLAM

## env - uv
```bash
# Create and activate the virtual environment
uv venv .mast3r-slam_venv --python 3.11
source .mast3r-slam_venv/bin/activate

# Clone the repo (do this before installing local packages)
git clone https://github.com/rmurai0610/MASt3R-SLAM.git --recursive
cd MASt3R-SLAM/
# if you've cloned the repo without --recursive, run this after cd MASt3R-SLAM/:
# git submodule update --init --recursive

# Install PyTorch with matching CUDA version
# Choose one of the following based on your system's CUDA toolkit:

# For CUDA 12.1 (or other CUDA 12.x versions like 12.4):
uv pip install torch==2.5.1 torchvision==0.20.1 torchaudio==2.5.1 --index-url https://download.pytorch.org/whl/cu121

# Ensure build tools are present in the environment
uv pip install --upgrade setuptools wheel cython

# IMPORTANT for CUDA custom kernel compilation:
module load cuda/12.1.1
# OPTIONAL: Verify cuda installation path
echo $CUDA_HOME
# Load compatible gcc version
module load compiler/gcc-10.1


# Install dependencies
uv pip install --no-build-isolation -e thirdparty/mast3r
uv pip install -e thirdparty/in3d
uv pip install --no-build-isolation -e .

# Optionally install torchcodec for faster mp4 loading
uv pip install torchcodec==0.1
```

## env - conda
```
conda create -n mast3r-slam python=3.11
conda activate mast3r-slam
```

Install pytorch with matching CUDA version following:
```
# CUDA 11.8
conda install pytorch==2.5.1 torchvision==0.20.1 torchaudio==2.5.1  pytorch-cuda=11.8 -c pytorch -c nvidia
# CUDA 12.1
conda install pytorch==2.5.1 torchvision==0.20.1 torchaudio==2.5.1 pytorch-cuda=12.1 -c pytorch -c nvidia
# CUDA 12.4
conda install pytorch==2.5.1 torchvision==0.20.1 torchaudio==2.5.1 pytorch-cuda=12.4 -c pytorch -c nvidia
```
### Clone the repo and install the dependencies.
```
git clone https://github.com/rmurai0610/MASt3R-SLAM.git --recursive
cd MASt3R-SLAM/
```
### if you've clone the repo without --recursive run
`git submodule update --init --recursive`
```
pip install -e thirdparty/mast3r
pip install -e thirdparty/in3d
pip install --no-build-isolation -e .
```


## checkpoints
Setup the checkpoints for MASt3R and retrieval.
# The license for the checkpoints and more information on the datasets used is written here.
```
mkdir -p checkpoints/
wget https://download.europe.naverlabs.com/ComputerVision/MASt3R/MASt3R_ViTLarge_BaseDecoder_512_catmlpdpt_metric.pth -P checkpoints/
wget https://download.europe.naverlabs.com/ComputerVision/MASt3R/MASt3R_ViTLarge_BaseDecoder_512_catmlpdpt_metric_retrieval_trainingfree.pth -P checkpoints/
wget https://download.europe.naverlabs.com/ComputerVision/MASt3R/MASt3R_ViTLarge_BaseDecoder_512_catmlpdpt_metric_retrieval_codebook.pkl -P checkpoints/
```
