# RPAFlow
This is the official implementation for RPAFlow, a deep coarse-to-fine network for 3D scene flow estimation on point clouds.

## Environment
Create a PyTorch environment using conda:
```
conda create -n rpaflow python=3.8
conda activate rpaflow
conda install pytorch==1.12.1 torchvision==0.13.1 cudatoolkit=11.3 -c pytorch
```

Install other dependencies:
```
pip install opencv-python open3d tqdm
```

Compile CUDA extensions for faster training and evaluation (optional):
```
cd csrc
python setup.py build_ext --inplace
```

## Data preprocess
For fair comparison with previous methods, we adopt the preprocessing steps in HPLFlowNet.
### FlyingThings3D
Download the [FlyingThings3D subset] (https://lmb.informatik.uni-freiburg.de/resources/datasets/SceneFlowDatasets.en.html)

You need to download the following part:

* Disparity
* Disparity occlusions
* Disparity change
* Optical flow
* Flow occlusions

Uncompress them into the same directory, ```RAW_DATA_PATH```. Run the following script for 3D reconstruction:
```
python data_preprocess/process_flyingthings3d_subset.py --raw_data_path RAW_DATA_PATH --save_path SAVE_PATH/FlyingThings3D_subset_processed_35m --only_save_near_pts
```

### KITTI
Download the [KITTI Scene Flow 2015 dataset] (https://www.cvlibs.net/datasets/kitti/eval_scene_flow.php)

You need to download the following part:
* Main data
* Calibration files

Then, uncompress them into the same directory, ```RAW_DATA_PATH```. Run the following script for 3D reconstruction:
```
python data_preprocess/process_kitti.py RAW_DATA_PATH SAVE_PATH/KITTI_processed_occ_final
```

## Getting started

### Training
The training process is done on FlyingThings3D subset. First, Set ```data_root``` in the configuration file to ```SAVE_PATH``` in the data preprocess section. Then, train the model with a quarter dataset:
```
python train.py --config conf/config_train.yaml
```

Afterward, finetune the model on the full dataset, with the pretrained model path ```ckpt_path```：
```
python train.py --config conf/config_train_fintune.yaml --weights ckpt_path
```

### Evaluation
We have upload one pretrained model in ```pretrain_weights```.
You could evaluate the model on FlyingThings3D:
```
python evaluate.py --config conf/config_evaluate.yaml --weights pretrain_weights/RPAFlow.pth
```

For evaluation on KITTI, you need to set ```data_root``` in the configuration file to ```SAVE_PATH``` in the data preprocess section. Then run:
```
python evaluate.py --config conf/config_evaluate_kitti.yaml --weights pretrain_weights/RPAFlow.pth
```







