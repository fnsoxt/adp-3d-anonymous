# ADP-3D: Solving Inverse Problems in Protein Space Using Diffusion-Based Priors

## Welcome to ADP-3D's Codebase!

ADP-3D (Atomic Denoising Priors for 3D Reconstruction) is a method for leveraging a pretrained protein diffusion model as a prior to solve 3D reconstruction problems.

![method](images/method_white.png)

The current implementation uses [Chroma](https://generatebiomedicines.com/chroma) as the pretrained model.

## Installation

Start by cloning the repo and, from the main directory, run:
```
conda create adp-3d-env python=3.9
conda activate adp-3d-env
pip install -r requirements.txt
conda install -c conda-forge ffmpeg
```

## Chroma API Key

Follow [this link](https://chroma-weights.generatebiomedicines.com/) to request your Chroma API key. Then, run:
```
python register.py --key YOUR_API_KEY
```

## Structure Completion

We provide a structure completion example with [PDB:8ok3](https://www.rcsb.org/structure/8OK3). To launch your experiment with, for example, a sub-sampling factor of 4, run:
```
python structure_completion.py --outdir /path/to/output/directory --cif data/cifs/8ok3.cif --fix-every 4
```

Once your experiment is finished, you can find the loss curves, RMSD curves and the final structure in the output directory.

We also provide our implementation of the [DPS](https://openreview.net/forum?id=OnD9zGAGT0k) method, which we found to perform worse than our [DiffPIR](https://yuanzhi-zhu.github.io/DiffPIR/)-based implementation.

## Distances to Structure

We provide an example for the "distances-to-structure" task with [PDB:8ok3](https://www.rcsb.org/structure/8OK3). To launch your experiment with, for example, 500 known pairwise distances, run:
```
python distances_to_structure.py --outdir /path/to/output/directory --cif data/cifs/8ok3.cif --n-distances 500 --lr-distance 0.4
```

## Model Refinement

We provide an example with [PDB:7PZT](https://www.rcsb.org/structure/7PZT). We generated the density map in [ChimeraX](https://www.cgl.ucsf.edu/chimerax/). An incomplete model was obtained with [ModelAngelo](https://github.com/3dem/model-angelo), using the default parameters. To launch your experiment, run:
```
python model_refinement.py --outdir /path/to/output/directory --mrc data/mrcs/7pzt_2.0A.mrc --ma-cif data/cifs/7pzt_MA_2.0A.cif --cif data/cifs/7pzt.cif
```

In the output directory, you will find a plot showing the completeness of the model vs. RMSD:
![rmsd](images/rmsd_ca_vs_completeness.png)

## Troubleshooting

*Failed to resolve 'chroma-weights.generatebiomedicines.com'*

You may not have access to the internet. If you are able to instantiate a Chroma model in a different configuration (e.g., on the login node of a cluster), you will see that the weights of the model (backbone model and design model) are stored in a temporary location (`/tmp/...` ending with `.pt`). To bypass the need for connecting to the internet, you can copy/paste these `.pt` files to a stable location and point to them with the following additional arguments:
```
--weights-backbone /path/to/weights/of/backbone/model --weights-design /path/to/weights/of/design/model
```
