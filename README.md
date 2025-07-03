# 3D Human Texture Model Generation from single image

## Result Table

| Model | Input | Output | Notes | Colab |
|-------|-------|--------|-------|-------|
| [SiTH](https://github.com/SiTH-Diffusion/SiTH) | RGB image | Textured human model | Uses OpenPose; may create distorted models with self-occlusions; segment body; GPU out-of-memory (OOM) issues. | |
| [PIFu](https://shunsukesaito.github.io/PIFu/) | RGB image | Textured human model | Trained model not found. | |
| [PSHuman](https://github.com/pengHTYX/PSHuman/) | RGB image | Textured human model | Uses diffusion model to hallucinate viewpoints, addresses self-occlusion; requires 24 GB GPU ([issue #4](https://github.com/pengHTYX/PSHuman/issues/4)); online demo unavailable. (The current model is trained at a resolution of 768, requiring over 40GB of VRAM. We are considering training a new model at a resolution of 512, which would allow it to run on an RTX 4090.) | |
| [HumanRef](https://github.com/eckertzhang/HumanRef) | RGB image | Textured human model | Diffusion-based; high GPU requirements; needs BLIP2 model; online demo unavailable. | |
| [SIFU](https://github.com/River-Zhang/SIFU) | RGB image | Textured human model | Supports diffusion-based texturing and editing; requires ≥16 GB GPU memory; online demo unavailable. My note for code replication 复现: [./SIFU_note.md](./SIFU_note.md) | N/A (Colab Python, CUDA version incompatible) |
| [IDOL](https://github.com/yiyuzhuang/IDOL) | RGB image | A-Pose textured human model | Generates A-Pose model; requires ≥24 GB GPU memory, potentially reducible ([pull #17](https://github.com/yiyuzhuang/IDOL/pull/17)). | |
| [SMPLitex](https://dancasas.github.io/projects/SMPLitex/index.html) | RGB image | A-Pose textured human model | Diffusion-based 3D human texture inference; generates A-Pose model. | |

## Todo List
- add HSMR and SMPLIFX
### Model Experiments
- [x] **SiTH**: Replicate ([GPU OOM issue](https://github.com/SiTH-Diffusion/SiTH))
- [x] **PIFu**: Replicate ([Pretrained model link expired, issue opened](https://shunsukesaito.github.io/PIFu/))
- [x] **SIFU**: Set up environment + write documentation
- [ ] **RSC-Net**: Run + write documentation ([GitHub](https://github.com/xuxy09/RSC-Net))
- [ ] Write environment setup documentation for all tested models

## Papers Without Code

- [SMPL Normal Map Is All You Need for Single-view Textured Human Reconstruction](http://arxiv.org/pdf/2506.12793v1)
- [CHROME: Clothed Human Reconstruction with Occlusion-Resilience and Multiview-Consistency from a Single Image](http://arxiv.org/pdf/2503.15671v1)
- [DeClotH: Decomposable 3D Cloth and Human Body Reconstruction from a Single Image](https://hygenie1228.github.io/DeClotH/)
- [Single-Image 3D Human Digitization with Shape-Guided Diffusion](https://human-sgd.github.io/)

## Related Papers

- [MVD-HuGaS: Human Gaussians from a Single Image via 3D Human Multi-view Diffusion Prior](http://arxiv.org/pdf/2503.08218v1)
- [Realistic Clothed Human and Object Joint Reconstruction from a Single Image](http://arxiv.org/pdf/2502.18150v2)
- [HumanGif: Single-View Human Diffusion with Generative Prior](http://arxiv.org/pdf/2502.12080v2)
- [Pippo: High-Resolution Multi-View Humans from a Single Image](http://arxiv.org/pdf/2502.07785v1)
- [GAS: Generative Avatar Synthesis from a Single Image](http://arxiv.org/pdf/2502.06957v1)
- [HuGDiffusion: Generalizable Single-Image Human Rendering via 3D Gaussian Diffusion](http://arxiv.org/pdf/2501.15008v1)
- [IDOL: Instant Photorealistic 3D Human Creation from a Single Image](http://arxiv.org/pdf/2412.14963v2)
- [PF-LHM: 3D Animatable Avatar Reconstruction from Pose-free Articulated Human Images](http://arxiv.org/pdf/2506.13766v1)
- [AdaHuman: Animatable Detailed 3D Human Generation with Compositional Multiview Diffusion](http://arxiv.org/pdf/2505.24877v1)
- [SVAD: From Single Image to 3D Avatar via Synthetic Data Generation with Video Diffusion and Data Augmentation](http://arxiv.org/pdf/2505.05475v1)
- [LHM: Large Animatable Human Reconstruction Model from a Single Image in Seconds](http://arxiv.org/pdf/2503.10625v1)
- [SIGMAN: Scaling 3D Human Gaussian Generation with Millions of Assets](http://arxiv.org/pdf/2504.06982v1)
- [HumanDreamer-X: Photorealistic Single-image Human Avatars Reconstruction via Gaussian Restoration](http://arxiv.org/pdf/2504.03536v1)
- [ICON](https://icon.is.tue.mpg.de/index.html)
