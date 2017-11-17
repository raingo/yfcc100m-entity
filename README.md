# YFCC100M Entity Dataset

The YFCC100M Entity Dataset contains about 460K unique images, associated with a mix of clean and noisy labels. The labels are all Wikipedia entities, in the hope to leverage semantic knowledges. It is suitable for the learning from noisy labels research. The images are collected from both ImageNet and YFCC100M. We provide the [previously released public URLs](https://aws.amazon.com/public-datasets/multimedia-commons/) for the YFCC100M dataset, and filenames for the images from ImageNet. The images are from three different domains, i.e., **Species**, **Sports**, and **Artifacts**. The labels from ImageNet are assumed all **clean**, while the labels from YFCC100M are regarded as **noisy**. Portion of the YFCC100M labels are cleaned via crowdsourcing platform **CrowdFlower**. To evaluate the performance of **learning from noisy labels**, please strictly follow our protocols details below.

If you end up using the dataset, we ask you to cite the following paper: [preprint](https://arxiv.org/abs/1703.02391)

Yuncheng Li, Jianchao Yang, Yale Song, Liangliang Cao, Jiebo Luo, and Li-Jia Li. "Learning from Noisy Labels with Distillation", ICCV 2017

If you have any question regarding the dataset, please contact: (or open an issue on GitHub)

  Yuncheng Li <yli@cs.rochester.edu>

# License
This dataset is provided to be used for approved non-commercial research
purposes. No personally identifying information is available in this dataset.

# Dataset

There are four directories under `data`, which corresponds to the four settings in the paper. Each directory contains 5 files, `full.txt`, `dev.txt`, `test.txt`, `clean.txt`, and `vocab.txt`. 

The `vocab.txt` list the name of the labels, which corresponds to a Wikipedia page. For example, `String_instrument` in the file `data/Artifacts/vocab.txt` means the entity explained in this Wikipedia page: [String_instrument](https://en.wikipedia.org/wiki/String_instrument). The other files have the format `link label source`.

`source=yfcc100m` means the image comes from YFCC100M dataset and the `link` is a public URL pointing to the resized version of the image.

`source=imagenet` means the image comes from ImageNet and the `link` is the file name of the image. To get the image, you have to register and download from [ImageNet](http://image-net.org/download-images)

The `label` is an index to the vocabulary file `vocab.txt`. For the `dev.txt` and `test.txt` files, the `label` may be `-1`, which means it is a background image.

The `clean.txt` is a subset of `full.txt`. `clean.txt` and `full.txt` means the `D_c` and `D` in the paper, respectively.

# Examples

`head data/Artifacts/vocab.txt`:

```
Yacht
Windmill
Wind_turbine
Wi-Fi
Wheelbarrow
Wetsuit
Webcam
Watermill
Water_wheel
Violin
```

`head data/Species-I/full.txt`:

```
n02212062/n02212062_6734.JPEG 206 imagenet
https://multimedia-commons.s3-us-west-2.amazonaws.com/data/images/3d5/432/3d5432fcae60bb456ed783fd778abfed.jpg 21 yfcc100m
https://multimedia-commons.s3-us-west-2.amazonaws.com/data/images/96f/79a/96f79a69125b21f9e7dd4a1fad8771d.jpg 76 yfcc100m
n02137549/n02137549_9284.JPEG 137 imagenet
https://multimedia-commons.s3-us-west-2.amazonaws.com/data/images/f0c/668/f0c66859b9ddf7165dffb774dc19434e.jpg 184 yfcc100m
https://multimedia-commons.s3-us-west-2.amazonaws.com/data/images/f5d/246/f5d246286d1ebcacb395427d085eee5.jpg 183 yfcc100m
https://multimedia-commons.s3-us-west-2.amazonaws.com/data/images/6b4/0e0/6b40e08b8cd89bdee6443b901c190.jpg 110 yfcc100m
n12685431/n12685431_2352.JPEG 92 imagenet
https://multimedia-commons.s3-us-west-2.amazonaws.com/data/images/757/4fb/7574fb31b485b0cea668b9cb46d71db1.jpg 138 yfcc100m
https://multimedia-commons.s3-us-west-2.amazonaws.com/data/images/175/9ad/1759ad7e9b756c7d2ee5959bce58b40.jpg 165 yfcc100m
```

# Protocol

In order to make the experiments comparable as much as possible, please follow the following guidelines:
1. Model training should be on `clean.txt` and `full.txt`.
2. `dev.txt` can be used only for selecting hyperparameters. Training on `dev.txt` is forbidden.
3. Final results should be reported on the `test.txt`, and `test.txt` should not be used to tune hyperparameters.
4. `mAP` should be used as evaluation metric:

```
import numpy as np
from sklearn.metrics import average_precision_score
# sklearn version: 0.18.1 (for reference)

aps = []
for i in range(vocab_size):
    aps.append(average_precision_score(this_labels, this_score))
mAP = np.mean(aps)
```
