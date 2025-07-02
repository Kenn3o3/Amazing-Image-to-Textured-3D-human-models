# SMPLIFY-X Implementation Note

## Prerequisites

### Install Required Python Packages
```bash
pip install pyyaml
pip install configargparse
pip install opencv-python
pip install omegaconf
pip install loguru
```

### Install CUDA and NVIDIA Toolkit
```bash
sudo apt install nvidia-cuda-toolkit
export CUDA_HOME=/usr/lib/cuda
```

### Install CUDA Samples
```bash
git clone https://github.com/NVIDIA/cuda-samples
export CUDA_SAMPLES_INC=/home/kenny/cuda-samples/Common
```

### Install torch-mesh-isect
```bash
git clone https://github.com/vchoutas/torch-mesh-isect
cd torch-mesh-isect
pip install -r requirements.txt
python setup.py install
```

#### Modify torch-mesh-isect Code
In the `torch-mesh-isect` codebase, update the BVH tree construction code as follows:
```cpp
// Construct the bvh tree
AT_DISPATCH_FLOATING_TYPES(
    triangles.scalar_type(), "bvh_tree_building", ([&] { // Changed: triangles.type() --> triangles.scalar_type()
      thrust::device_vector<BVHNode<scalar_t>> leaf_nodes(num_triangles);
      thrust::device_vector<BVHNode<scalar_t>> internal_nodes(num_triangles - 1);
      auto triangle_float_ptr = triangles.data_ptr<scalar_t>(); // Changed: data<scalar_t>() --> data_ptr<scalar_t>()
```

## Code Modifications for SMPLIFY-X

### Update Imports in `fitting.py`
Add the following imports:
```python
from human_body_prior.tools.model_loader import load_model
from human_body_prior.models.vposer_model import VPoser
```

### Modify VPoser Loading
Replace:
```python
vposer, _ = load_vposer(vposer_ckpt, vp_model='snapshot')
```
with:
```python
vposer, _ = load_model(vposer_ckpt, model_code=VPoser, remove_words_in_model_weights='vp_model.', disable_grad=True)
```

### Update Body Pose Decoding
In `fitting.py`, replace all instances of:
```python
body_pose = vposer.decode(pose_embedding, output_type='aa').view(1, -1) if use_vposer else None
```
with:
```python
body_pose = (vposer.decode(pose_embedding).get('pose_body')).reshape(1, -1) if use_vposer else None
```

## OpenPose Keypoint Extraction

For best results with SMPL-X, extract body (BODY_25), hand, and face keypoints using OpenPose.

### Install OpenPose
```bash
git clone https://github.com/CMU-Perceptual-Computing-Lab/openpose.git
cd openpose
git submodule update --init --recursive --remote
mkdir build/
cd build/
sudo apt update
sudo apt install -y libgflags-dev libgoogle-glog-dev libprotobuf-dev protobuf-compiler libopencv-dev libboost-all-dev libhdf5-dev libatlas-base-dev
cmake ..
make -j`nproc`
cd ..
```

### Extract Keypoints
Run OpenPose to extract keypoints and save them as JSON files or images. Use the `--net_resolution "320x176"` flag for better performance.

#### Extract Body, Face, and Hand Keypoints Together
```bash
./build/examples/openpose/openpose.bin --image_dir data/images/ --face --hand --write_json output_jsons/
./build/examples/openpose/openpose.bin --image_dir data/images/ --face --hand --write_images output_images/
```

#### Extract Face and Hand Keypoints Separately
For improved accuracy, process face and hand keypoints separately and combine the JSON outputs:
```bash
./build/examples/openpose/openpose.bin --image_dir data/images/ --face --write_json output_jsons_face/ --net_resolution "320x176"
./build/examples/openpose/openpose.bin --image_dir data/images/ --face --write_images output_images_face/ --net_resolution "320x176"
./build/examples/openpose/openpose.bin --image_dir data/images/ --hand --write_json output_jsons_hand/ --net_resolution "320x176"
./build/examples/openpose/openpose.bin --image_dir data/images/ --hand --write_images output_images_hand/ --net_resolution "320x176"
```

### Tips
- Combine JSON files from separate face and hand extractions for comprehensive keypoint data.
- Download sample images and keypoints from: [SMPLIFY-X GitHub Issue #46](https://github.com/vchoutas/smplify-x/issues/46).

Example script for merging JSON files:

```py
import json
import os
import sys

def merge_json(hand_path, face_path, output_path):
    with open(hand_path, 'r') as f:
        hand_json = json.load(f)
    with open(face_path, 'r') as f:
        face_json = json.load(f)
    
    new_json = {
        "version": hand_json["version"],
        "people": []
    }
    
    hand_person = hand_json["people"][0]
    face_person = face_json["people"][0]
    
    new_person = {
        "person_id": hand_person["person_id"],
        "pose_keypoints_2d": hand_person["pose_keypoints_2d"],
        "face_keypoints_2d": face_person["face_keypoints_2d"],
        "hand_left_keypoints_2d": hand_person["hand_left_keypoints_2d"],
        "hand_right_keypoints_2d": hand_person["hand_right_keypoints_2d"],
        "pose_keypoints_3d": [],
        "face_keypoints_3d": [],
        "hand_left_keypoints_3d": [],
        "hand_right_keypoints_3d": []
    }
    
    new_json["people"].append(new_person)
    
    with open(output_path, 'w') as f:
        json.dump(new_json, f)

def main():
    if len(sys.argv) != 4:
        print("Usage: python merge.py output_jsons_hand output_jsons_face output_jsons_keypoints")
        sys.exit(1)
    
    hand_dir = sys.argv[1]
    face_dir = sys.argv[2]
    output_dir = sys.argv[3]
    
    os.makedirs(output_dir, exist_ok=True)
    
    for filename in os.listdir(hand_dir):
        if filename.endswith('.json'):
            hand_path = os.path.join(hand_dir, filename)
            face_path = os.path.join(face_dir, filename)
            output_path = os.path.join(output_dir, filename)
            
            if os.path.exists(face_path):
                merge_json(hand_path, face_path, output_path)
            else:
                print(f"Warning: {face_path} does not exist, skipping {filename}")

if __name__ == "__main__":
    main()
```