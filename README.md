# COCO-Stuff dataset v1.0
[Holger Caesar](http://www.it-caesar.com), [Jasper Uijlings](http://homepages.inf.ed.ac.uk/juijling), [Vittorio Ferrari](http://calvin.inf.ed.ac.uk/members/vittoferrari)

## Overview
<img src="http://calvin.inf.ed.ac.uk/wp-content/uploads/data/cocostuffdataset/cocostuff-examples.png" alt="COCO-Stuff example annotations" width="100%">
Welcome to this release of the COCO-Stuff [1] dataset. COCO-Stuff augments the popular COCO [2] dataset with pixel-level stuff annotations. These annotations can be used for scene understanding tasks like semantic segmentation, object detection and image captioning.

## Overview
- [Highlights](#highlights)
- [Dataset](#dataset)
- [Semantic Segmentation Models](#semantic-segmentation-models)
- [Annotation Tool](#annotation-tool)
- [Misc](#misc)

## Highlights
- 10,000 complex images from [2]
- Dense pixel-level annotations
- 80 thing and 91 stuff classes
- Instance-level annotations for things from [2]
- Complex spatial context between stuff and things
- 5 captions per image from [2]

## Dataset
Filename | Description | Size
--- | --- | ---
[cocostuff-v1.0.zip](http://calvin.inf.ed.ac.uk/wp-content/uploads/data/cocostuffdataset/cocostuff-data-v1.0.zip) | COCO-Stuff dataset version 1.0, including images and annotations | 2.6 GB
[cocostuff-readme.txt](https://raw.githubusercontent.com/nightrome/cocostuff/master/README.md) | This document | 6.5 KB

### Usage
To use the COCO-Stuff dataset, please follow these steps:

1. Download or clone this repository using git: `git clone https://github.com/nightrome/cocostuff.git`
2. Open the dataset folder in your shell: `cd cocostuff`
3. Download and unzip the dataset:
  - `wget --directory-prefix=downloads http://calvin.inf.ed.ac.uk/wp-content/uploads/data/cocostuffdataset/cocostuff-data-v1.0.zip`
  - `unzip downloads/cocostuff-data-v1.0.zip -d dataset/`
4. Add the code folder to your Matlab path: `startup();`
5. Run the demo script in Matlab `demo_cocoStuff();`
6. The script displays an image, its thing, stuff and thing+stuff annotations, as well as the image captions.

### File format
The COCO-Stuff annotations are stored in separate .mat files per image. These files follow the same format as used by Tighe et al.. Each file contains the following fields:
- *S:* The pixel-wise label map of size [height x width]. For use in Matlab all label indices start from 1
- *names:* The names of the 172 classes in COCO-Stuff. 1 is the class 'unlabeled', 2-81 are things and 82-172 are stuff classes.
- *captions:* Image captions from [2] that are annotated by 5 distinct humans on average.
- *regionMapStuff:* A map of the same size as S that contains the indices for the approx. 1000 regions (superpixels) used to annotate the image.
- *regionLabelsStuff:* A list of the stuff labels for each superpixel. The indices in regionMapStuff correspond to the entries in regionLabelsStuff.

## Semantic Segmentation Models
To encourage further research of stuff and things we provide the trained semantic segmentation model (see Sect. 4.4 in [1]).

### DeepLab
Use the following steps to download and setup the DeepLab [4] semantic segmentation model trained on COCO-Stuff. It requires [deeplab-public-ver2](https://bitbucket.org/aquariusjay/deeplab-public-ver2) and is built on [Caffe](caffe.berkeleyvision.org):

1. Download deeplab-public-ver2: `git submodule update --init models/deeplab-public-ver2`
2. Compile and configure deeplab-public-ver2 following the [author's instructions](https://bitbucket.org/aquariusjay/deeplab-public-ver2). Depending on your system setup you might have to install additional packages, but a minimum setup could look like this:
  - `cd models/deeplab-public-ver2`
  - `cp Makefile.config.example Makefile.config`
  - `make`
  - `cd ../..`
3. Download and unzip the model:
  - `wget --directory-prefix=downloads http://calvin.inf.ed.ac.uk/wp-content/uploads/data/cocostuffdataset/cocostuff-deeplab.zip`
  - `unzip downloads/cocostuff-deeplab.zip -d models/deeplab-public-ver2/`
4. Configure the COCO-Stuff dataset:
  - Create a symbolic link to the images: `mkdir models/deeplab-public-ver2/cocostuff/data && ln -s ../../../../dataset/images models/deeplab-public-ver2/cocostuff/data/images`
  - Convert the annotations by running the Matlab script: `convertAnnotationsDeeplab();`
5. Run `cd models/deeplab-public-ver2 && ./run_cocostuff.sh && cd ../..` to train and test the network on COCO-Stuff.

## Annotation Tool
In [1] we present a simple and efficient stuff annotation tool which was used to annotate the COCO-Stuff dataset. It uses a paintbrush tool to annotate SLICO superpixels (precomputed using the [code](http://ivrl.epfl.ch/files/content/sites/ivrg/files/supplementary_material/RK_SLICsuperpixels/SLIC_mex.zip) of [Achanta et al.](http://ivrl.epfl.ch/research/superpixels)) with stuff labels. These annotations are overlaid with the existing pixel-level thing annotations from COCO.
We provide a basic version of our annotation tool:
- Run the user interface in Matlab: `CocoStuffAnnotator();`
- The tool uses the images, regions and imageLists in `annotator/data/input`. The superpixel regions need to be provided by external tools and follow the format of the example files.
- The tool writes the .mat label files to `annotator/data/output/annotations`.
- To create a .png preview of the annotations, run `annotator/code/exportImages.m` in Matlab. The previews will be saved to `annotator/data/output/preview`.

## Misc
### References
- [1] [COCO-Stuff: Thing and Stuff Classes in Context](https://arxiv.org/abs/1612.03716)<br />
H. Caesar, J. Uijlings, V. Ferrari,<br />
In *arXiv preprint arXiv:1612.03716*, 2016.<br />

- [2] [Microsoft COCO: Common Objects in Context](https://arxiv.org/abs/1405.0312)<br />
T.-Y. Lin, M. Maire, S. Belongie et al.,<br />
In *European Conference in Computer Vision* (ECCV), 2014.<br />

- [3] [Fully convolutional networks for semantic segmentation](http://www.cv-foundation.org/openaccess/content_cvpr_2015/html/Long_Fully_Convolutional_Networks_2015_CVPR_paper.html)<br />
J. Long, E. Shelhammer and T. Darrell,<br />
in *Computer Vision and Pattern Recognition* (CVPR), 2015.<br />

- [4] [Semantic image segmentation with deep convolutional nets and fully connected CRFs](https://arxiv.org/abs/1412.7062)<br />
L.-C. Chen, G. Papandreou, I. Kokkinos, K. Murphy and A. L. Yuille,<br />
In *International Conference on Learning Representations* (ICLR), 2015.<br />

### Licensing
COCO-Stuff is a derivative work of the COCO dataset. The authors of COCO do not in any form endorse this work. Different licenses apply:
- COCO images: [Flickr Terms of use](http://mscoco.org/terms_of_use/)
- COCO annotations: [Creative Commons Attribution 4.0 License](http://mscoco.org/terms_of_use/)
- COCO-Stuff annotations & code: [Creative Commons Attribution 4.0 License](https://creativecommons.org/licenses/by/4.0/legalcode)

### Contact
If you have any questions regarding this dataset, please contact us at holger.caesar-at-ed.ac.uk.
