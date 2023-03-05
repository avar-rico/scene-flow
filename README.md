**Update:**

* Self-supervised loss is release in model.py(multiScaleChamferSmoothCurvature).
You can train the self-supervised model by using train_self.py.
* Update PointConvFlow to compute the patch-to-patch cost volume.
* Update a updated model pretrain weight to obtain a better result(*<strong>EPE3D: 0.0463 vs 0.0588</strong>*) than the original paper.



## Prerequisities
Our model is trained and tested under:
* Python 3.6.9
* NVIDIA GPU + CUDA CuDNN
* PyTorch (torch == 1.5)
* scipy
* tqdm
* sklearn
* numba
* cffi
* pypng
* pptk





