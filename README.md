# MolNexTR
This is the official code of the following paper, "MolNexTR: A Generalized Deep Learning Model for Molecular Image Recognition".

## Highlights
<p align="justify">
In this work, We propose MolNexTR, a novel graph generation model. The model follows the encoder-decoder architecture, takes three-channel molecular images as input, outputs molecular graph structure prediction, and can be easily converted to SMILES. We aim to enhance the robustness and generalization of the molecular structure recognition model by enhancing the feature extraction ability of the model and the augmentation strategy, to deal with any molecular images that may appear in the real literature.

[comment]: <> ()
![visualization](figure/arch.png)
<div align="center">
Overview of our MolNexTR model.
</div> 

## Using the code and the model
### Using the code
Clone the following repositories:
```
git clone https://github.com/CYF2000127/MolNexTR
```
### Example usage of the model
1. Install requirements:
```
pip install -r requirements.txt
```

2. Download the model checkpoint from our [Hugging Face Repo](https://huggingface.co/datasets/CYF200127/MolNexTR/blob/main/molnextr_best.pth) or Zenodo Repo: [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.13304899.svg)](https://doi.org/10.5281/zenodo.13304899) and put in your own path 

3. Run the following code to predict molecular images:
```python
import torch
from MolNexTR import molnextr
Image = './examples/1.png'
Model = './checkpoints/molnextr_best.pth'
device = torch.device('cpu')
model = molnextr(Model, device)
predictions = model.predict_final_results(Image)
print(predictions)
```
or use [`prediction.ipynb`](prediction.ipynb). You can also change the image and model path to your own images and models.

The input is a molecular image 
![visualization](examples/1.png)
<div align="center",width="50">
Example input molecular image.
</div> 
The output dictionary includes the atom sets, bond sets, predicted MolFile, and predicted SMILES:

``` 
{
    'atom_sets':  [
                  {'atom_number': '0', 'symbol': 'Ph', 'coords': (0.143, 0.349)},
                  {'atom_number': '1', 'symbol': 'C', 'coords': (0.286, 0.413)},
                  {'atom_number': '2', 'symbol': 'C', 'coords': (0.429, 0.349)}, ... 
                  ],
    'bonds_sets': [
                  {'atom_number': '0', 'bond_type': 'single', 'endpoints': (0, 1)},
                  {'atom_number': '1', 'bond_type': 'double', 'endpoints': (1, 2)}, 
                  {'atom_number': '1', 'bond_type': 'single', 'endpoints': (1, 5)}, 
                  {'atom_number': '2', 'bond_type': 'single', 'endpoints': (2, 3)}, ...
                  ],
    'predicted_molfile': '2D\n\n 11 12  0  0  0  0  0  0  0  0999 V2000 ...',
    'predicted_smiles': 'COC1CCCc2oc(-c3ccccc3)cc21'
}   
```




## Experiments

### Requirements
```
pip install -r requirements.txt
```

### Data preparation
For training and inference, please download the following datasets to your own path.
#### Training datasets
1. **Synthetic:**  [PubChem](https://www.dropbox.com/s/mxvm5i8139y5cvk/pubchem.zip?dl=0)
2. **Realistic:**  [USPTO](https://www.dropbox.com/s/3podz99nuwagudy/uspto_mol.zip?dl=0)

#### Testing datasets
1. **Synthetic:**  [Indigo, ChemDraw](https://huggingface.co/datasets/CYF200127/MolNexTR/blob/main/synthetic.zip)
2. **Realistic:**  [CLEF, UOB, USPTO, JPO, Staker, ACS](https://huggingface.co/datasets/CYF200127/MolNexTR/blob/main/real.zip) 
3. **Perturbed by IMG transform:** [CLEF, UOB, USPTO, JPO, Staker, ACS](https://huggingface.co/datasets/CYF200127/MolNexTR/blob/main/perturb_by_imgtransform.zip)
4. **Perturbed by curved arrows:** [CLEF, UOB, USPTO, JPO, Staker, ACS](https://huggingface.co/datasets/CYF200127/MolNexTR/blob/main/perturb_by_arrows.zip)


### Train
Run the following command:
```
sh ./exps/train.sh
```
The default batch size was set to 256. And it takes about 20 hours to train with 10 NVIDIA RTX 3090 GPUs. 

### Inference
Run the following command:
```
sh ./exps/eval.sh
```
The default batch size was set to 32 with a single NVIDIA RTX 3090 GPU.
The outputs include the main metrics we used, such as SMILES exact matching accuracy and Graph exact matching accuracy.

### Prediction
Run the following command:
```
python prediction.py --model_path your_model_path --image_path your_image_path
```
### Visualization
Use [`visualization.ipynb`](visualization.ipynb) to visualize the ground truths and the predictions.

We also show some qualitative results of our method below:

![visualization](figure/vs1.png)
<div align="center">
Qualitative results of our method on ACS.

![visualization](figure/vs3.png)
Qualitative results of our method on some hand-drawn molecular images.
</div> 

