# SIFU Code Guide to output .obj mesh from a single image

**Step 1.** Follow the instructions in the [HSMR.ipynb](HSMR.ipynb) notebook and run to obtain the .npz files (stored in a folder called `npzfiles`) extracted from the images. You can use the Google Colab they provided [here](https://colab.research.google.com/drive/1RDA9iKckCDKh_bbaKjO8bQ0-Lv5fw1CB?usp=sharing#scrollTo=crnCfKf6zYWs).

**Step 2.** Run the following python script:
```py
import os

def main():
    import os
    npz_files = []
    for root, dirs, files in os.walk("npzfiles"):
        for file_name in files:
            if file_name.lower().endswith('.npz'):
                file_path = os.path.abspath(os.path.join(root, file_name))
                print("python /home/kenny/HSMR/exp/misc/export_mesh.py --input-path ", file_path, " --outputs-root /home/kenny/HSMR/output_mesh")
                npz_files.append(file_path)


if __name__ == '__main__':
    main()
```
