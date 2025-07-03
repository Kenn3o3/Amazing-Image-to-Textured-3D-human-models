# 3D Human Texture Model Generation from single image

## Replication Table for Image to Textured human model

| ToTest | Model | Input | Output | Notes | Colab |
|-------|-------|-------|--------|-------|-------|
| [x] | [SIFU](https://github.com/River-Zhang/SIFU) ! | RGB image | Textured human model | Supports diffusion-based texturing and editing; requires ≥16 GB GPU memory; online demo unavailable. My note for code replication 复现: [./SIFU_note.md](./SIFU_note.md) | N/A (Colab Python, CUDA version incompatible) |
| [x] | [SiTH](https://github.com/SiTH-Diffusion/SiTH) !* | RGB image | Textured human model | Uses OpenPose; may create distorted models with self-occlusions; segment body; GPU out-of-memory (OOM) issues. Uses diffusion model to hallucinate viewpoints, addresses self-occlusion; Requires 24 GB GPU. Just follow instructions. | N/A |
| [x] | [PSHuman](https://github.com/pengHTYX/PSHuman/) ! | RGB image | Textured human model | ([issue #4](https://github.com/pengHTYX/PSHuman/issues/4)); huggingface demo unavailable. (The current model is trained at a resolution of 768, requiring over 40GB of VRAM. We are considering training a new model at a resolution of 512, which would allow it to run on an RTX 4090.). Just follow instructions. | N/A |
| [x] | [HumanRef](https://github.com/eckertzhang/HumanRef) ! | RGB image | Textured human model | Diffusion-based; high GPU requirements; needs BLIP2 model; online demo unavailable. | N/A |
| [x] | [HumanGif](https://github.com/skhu101/HumanGif) | RGB image | Textured human model | It was tested on H100 GPU | N/A |
| [] | [LHM](https://github.com/aigc3d/LHM) | RGB image | Video / Textured human GS model | Honestly, the geometric properties of the 3D Gaussian are quite poor. The 3D Gaussian is designed for rendering, not for hard mesh. Even when using Poisson reconstruction, the resulting geometry is still not aesthetically pleasing. So, we do not provide a convert function to extract a obj file from the generated gaussian model. You can run the entire pipeline on 14 GB GPUs. Demo available on [HuggingFace](https://huggingface.co/spaces/3DAIGC/LHM) | N/A |
| [x] | [Humansplat](https://github.com/humansplat/humansplat) | RGB image | Textured human model |  HPS (Human Pose and Shape) Estimation to output .obj file| N/A |
| [] | [Tech](https://github.com/huangyangyi/TeCH) | RGB image | Textured human model | Requires 2*32G GPU memory, need addtiional configuration | N/A |
| [-] | [PIFu](https://shunsukesaito.github.io/PIFu/) ! | RGB image | Textured human model | Trained model not found. | N/A |
| |  |  |  |  | |

- !: Highly relevant
- *: May not work well for any poses
- A related Blender add-on: https://github.com/kwan3854/CEB_ECON

## Other related Papers
- [RSC-Net: 3D Human Pose, Shape and Texture from Low-Resolution Images and Videos](https://arxiv.org/pdf/2103.06498), [[code]](https://github.com/xuxy09/RSC-Net)
- https://github.com/rlczddl/awesome-3d-human-reconstruction?tab=readme-ov-file#texture
- [SMPL Normal Map Is All You Need for Single-view Textured Human Reconstruction](http://arxiv.org/pdf/2506.12793v1)
- [CHROME: Clothed Human Reconstruction with Occlusion-Resilience and Multiview-Consistency from a Single Image](http://arxiv.org/pdf/2503.15671v1)
- [DeClotH: Decomposable 3D Cloth and Human Body Reconstruction from a Single Image](https://hygenie1228.github.io/DeClotH/)
- [Single-Image 3D Human Digitization with Shape-Guided Diffusion](https://human-sgd.github.io/)
- [Pippo: High-Resolution Multi-View Humans from a Single Image](http://arxiv.org/pdf/2502.07785v1), [[Project Page]](https://yashkant.github.io/pippo/)
- [GAS: Generative Avatar Synthesis from a Single Image](http://arxiv.org/pdf/2502.06957v1), [[Project Page]](https://humansensinglab.github.io/GAS/)
- [SVAD: From Single Image to 3D Avatar via Synthetic Data Generation with Video Diffusion and Data Augmentation](http://arxiv.org/pdf/2505.05475v1), [[Project  Page]](https://yc4ny.github.io/SVAD/)
- [SIGMAN: Scaling 3D Human Gaussian Generation with Millions of Assets](http://arxiv.org/pdf/2504.06982v1), [no code]
- [HumanDreamer-X: Photorealistic Single-image Human Avatars Reconstruction via Gaussian Restoration](http://arxiv.org/pdf/2504.03536v1), [[Project Page]](https://humandreamer-x.github.io/), [no code]
- [Zeroavatar: Zero-shot 3d avatar generation from a single image](https://arxiv.org/pdf/2305.16411), [no code]
- [MVD-HuGaS: Human Gaussians from a Single Image via 3D Human Multi-view Diffusion Prior](http://arxiv.org/pdf/2503.08218v1), [no code]
- [Realistic Clothed Human and Object Joint Reconstruction from a Single Image](http://arxiv.org/pdf/2502.18150v2), [no code]
- [AdaHuman: Animatable Detailed 3D Human Generation with Compositional Multiview Diffusion](http://arxiv.org/pdf/2505.24877v1), [[Project Page]](https://nvlabs.github.io/AdaHuman/), [no code]
- [HuGDiffusion: Generalizable Single-Image Human Rendering via 3D Gaussian Diffusion](http://arxiv.org/pdf/2501.15008v1), [no code]
- [PF-LHM: 3D Animatable Avatar Reconstruction from Pose-free Articulated Human Images](http://arxiv.org/pdf/2506.13766v1), [no code]

- 

### Table for other related works

| ToCheck | Model | Input | Output | Notes | Colab |
|-------|-------|-------|--------|-------|-------|
| [] | [ICON](https://icon.is.tue.mpg.de/index.html) | RGB Image | half-textured clothed human model | Author referred to PiFu and TeCH for full textured model | [notebook](https://colab.research.google.com/drive/1-AWeWhPvCTBX0KfMtgtMk10uPU05ihoA?usp=sharing) |
| |  |  |  |  | |
| [] | [IDOL](https://github.com/yiyuzhuang/IDOL) | RGB image | A-Pose textured human model | Generates A-Pose model; requires ≥24 GB GPU memory, potentially reducible ([pull #17](https://github.com/yiyuzhuang/IDOL/pull/17)). | N/A |
| [] | [SMPLitex](https://dancasas.github.io/projects/SMPLitex/index.html) | RGB image | A-Pose textured human model | Diffusion-based 3D human texture inference; generates A-Pose model. | N/A |