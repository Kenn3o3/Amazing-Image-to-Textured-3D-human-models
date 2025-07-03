# SIFU Code Replication Guide

This guide provides step-by-step instructions to set up the environment for running the SIFU project. Follow these steps carefully to ensure a successful setup.

## Step 1: Install CUDA 11.7
1. Download and install CUDA 11.7 toolkit:
```bash
wget https://developer.download.nvidia.com/compute/cuda/11.7.0/local_installers/cuda_11.7.0_515.43.04_linux.run
chmod +x cuda_11.7.0_515.43.04_linux.run
sudo ./cuda_11.7.0_515.43.04_linux.run --toolkit --silent --override --toolkitpath=/usr/local/cuda-11.7
```
2. Verify CUDA installation:
```bash
ls /usr/local | grep cuda
```
   - Expected output: `cuda-11.7`
   - If not found, ensure the installation completed without errors and check `/usr/local` permissions.

## Step 2: Install CUB for PyTorch3D
1. Download and extract CUB 1.10.0:
```bash
curl -LO https://github.com/NVIDIA/cub/archive/1.10.0.tar.gz
tar xzf 1.10.0.tar.gz
mv cub-1.10.0 cub
rm 1.10.0.tar.gz
```
   - This places the `cub` directory in your working directory for PyTorch3D compatibility.

## Step 3: Install System Dependencies
1. Update package lists and install required packages:
```bash
sudo apt-get update
sudo apt-get install -y libeigen3-dev ffmpeg
```
   - Ensure no errors occur during installation. If errors arise, check your package manager or internet connection.

## Step 4: Set Up Conda Environment
1. Create a Python 3.8 conda environment named `sifu`:
```bash
conda create -n sifu python=3.8 -y
```
2. Activate the environment:
```bash
conda activate sifu
```
   - Ensure the environment is activated before proceeding to the next steps.

3. Set up Environment Variables:

```bash
export PATH=/usr/local/cuda-11.7/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-11.7/lib64:$LD_LIBRARY_PATH
export CUDA_HOME=/usr/local/cuda-11.7
export CUB_HOME=/home/kenny/cub/cub #change to corresponding path

nvcc --version # check if cuda version is set to 11.7
```

## Setup repo:

1. Clone github repo

```bash
git clone https://github.com/River-Zhang/SIFU.git
cd SIFU
pip install huggingface_hub
```

2. Download checkpoint files

Download checkpoints from https://drive.usercontent.google.com/download?id=13rNSmQI_VaMtwlMBSUaxEGybzJEl5KTi&export=download&authuser=0 and place it to ./SIFU/data/ckpt

3. Download the smpl_related data with the script (placed in the ./SIFU folder)

```
./fetch_hps.sh
./fetch_data.sh
```

There maybe some corrupted/missing files. You can download yourself, for example:

https://huggingface.co/lilpotat/pytorch3d/blob/c0f00775a655a0c6884184fc0a8564133592a5c2/smplx_extra_joints.yaml --> /home/kenny/SIFU/data/HPS/pixie_data/smplx_extra_joints.yaml

https://huggingface.co/lilpotat/pytorch3d/blob/8607853c732294f6b5c6922a56aa07821669c955/SMPLX_to_J14.pkl --> /home/kenny/SIFU/data/HPS/pixie_data/SMPLX_to_J14.pkl

https://huggingface.co/lilpotat/pytorch3d/blob/d4f107a6f00a816d50f6791ec28b62b2a006ce59/pixie_model.tar --> /home/kenny/SIFU/data/HPS/pixie_data/pixie_model.tar

```py
import os
import shutil
import glob
from huggingface_hub import HfApi, hf_hub_download

def download_smpl_data():
    repo_id = "lilpotat/pytorch3d"
    local_base_dir = "data/smpl_related/smpl_data"

    os.makedirs(local_base_dir, exist_ok=True)
    print("Current working directory:", os.getcwd())
    print("Saving files to:", os.path.abspath(local_base_dir))

    api = HfApi()
    
    all_files = api.list_repo_files(repo_id)
    smpl_data_files = [f for f in all_files if f.startswith("smpl_data/")]

    for file_path in smpl_data_files:
        relative_path = file_path[len("smpl_data/"):]
        local_file_path = os.path.join(local_base_dir, relative_path)

        if not os.path.exists(local_file_path):
            cached_file = hf_hub_download(repo_id, filename=file_path)
            local_dir = os.path.dirname(local_file_path)
            os.makedirs(local_dir, exist_ok=True)
            try:
                shutil.copy(cached_file, local_file_path)
                print(f"Successfully copied {file_path} to {local_file_path}")
            except Exception as e:
                print(f"Error copying {file_path} to {local_file_path}: {e}")

    print("Files in local_base_dir:", glob.glob(os.path.join(local_base_dir, "*")))

if __name__ == "__main__":
    download_smpl_data()
```

## Step 5: Install Python Dependencies
1. Install required Python packages:
```bash
pip install torch==1.13.0+cu117 torchvision==0.14.0+cu117 torchaudio==0.13.0 --extra-index-url https://download.pytorch.org/whl/cu117
pip install rembg[gpu] numpy==1.24.4
```
   - **Note**: Ensure CUDA 11.7 is correctly installed, as `torch==1.13.0+cu117` requires it.
   - If you encounter issues with `pytorch3d`, verify that the CUB library is in the working directory.

## Step 6: Check versions

```bash
python --version
python -c "import torch; print('PyTorch version:', torch.__version__)"
python -c "import torch; print('CUDA available:', torch.cuda.is_available())"
python -c "import torch; print('CUDA version:', torch.version.cuda)"

```
## Step 7: Modify and install pip -r requirements.txt

Make the following modifications in requirements.txt:
```
17: ipywidgets>=7.6.5
...
23: PyMCubes==0.1.4
...
33: protobuf==3.20.0
34: pymeshlab==2022.2.post4
35: dataclasses>=0.6
36: # numpy==1.24.4
37: # git+https://github.com/facebookresearch/pytorch3d.git@v0.7.2
38: git+https://github.com/YuliangXiu/neural_voxelization_layer.git
39: # git+https://github.com/NVIDIAGameWorks/kaolin.git #remove this line
40: # git+https://github.com/YuliangXiu/rembg.git #remove this line
```

```bash
pip install pip==24.0
pip install -r requirements.txt
pip install git+https://github.com/facebookresearch/pytorch3d.git@v0.7.2
pip install git+https://github.com/NVIDIAGameWorks/kaolin.git
```
