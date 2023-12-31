# MultiPropReg

This is the official code for the paper "Learning Deformable Image Registration from Optimization: Perspective, Modules, Bilevel Training and Beyond"


# Instructions

There are three methods to utilize the MultiPropReg library:

1. Clone this repository and install the requirements listed in 'setup.py'.
2. Install directly using pip.
3. Download the Docker image.

When installing via pip, please use this command to install MultiPropReg. 

```
pip install MultiPropReg==0.1.1
```
The format of the instruction you should enter in the terminal is as follows:


```
python -m MultiPropReg.scripts.test \
        --model /path/to/pretrained_model_file  \
        --atlas /path/to/atlas_file  \
        --image_path /path/to/test_image_path_file  \
        --result_save /path/to/result_save_file \
        --gpu 0 \
        --hyper_parameters 10,15,3.2,0.8
```

Each of the above parameters can be customized.

When you want to install the docker image, enter the following command in the terminal:

```
docker pull meiyunan/multipropreg:v2
```

Before using the pulled Docker image, you may need to activate the PyTorch environment first using the following command:

```
bash
source activate pytorch
```

The instruction format is essentially the same as that provided in scripts/README.md.


## Pre-trained models

Our pre-trained models are available here: https://pan.baidu.com/s/1BoCwtYANGkbRwDt8IBFHcQ?pwd=dimu .

## Training

If you would like to train your own model, you will likely need to customize some of the data-loading code in `MultiPropReg/datagenerators.py` for your own datasets and data formats. Training data can be in the NII, MGZ, or npz (numpy) format, and it's assumed that each npz file in your data list has a `vol` parameter, which points to the image data to be registered, and an optional `seg` variable, which points to a corresponding discrete segmentation (for semi-supervised learning). The training data we provide has been skull removed and affine registered, if you need to process your own data, you can use the tools we provide to do the data processing.

We have defined three training modes: 
        `train_src_arcitecture.py`: architecture-search training; 
        `train_src_hyper.py`: hyper parameter-search training (without segmentation data);
        `train_src_hyper_seg.py`: hyper parameter-search training (with segmentation data);

Detailed usage instructions can be found in scripts/README.md.

## Testing

If you want to test the training results, you can use our example:

```
python MultiPropReg/scripts/test.py  \
        --model MultiPropReg/models/pretrained_model_epoch400.ckpt \
        --atlas MultiPropReg/data/1yr_t1_atlas.nii  \
        --atlas_seg  /path/to/atlas_seg_file  \
        --image_path MultiPropReg/data/divided_dataset/Test_Affine  \
        --result_save MultiPropReg/output_test_400  \
        --gpu 0
        --hyper_parameters
```

If you have a dataset with segmented images, you have the option to manually specify the `atlas_seg` parameter. Additionally, we offer a selection of pre-trained models for a rapid assessment of registration outcomes.
Detailed usage instructions can be found in MultiPropReg/scripts/README.md.