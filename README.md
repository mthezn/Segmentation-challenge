This dataset consists of segmented images from Mars terrain. Each image is paired with a mask representing the class of each pixel. 

 
Dataset Details
Image Size: 64x128
Color Space: Grayscale (1 channel)
Input Shape: (64, 128)
File Format: npz (Numpy archive)
Number of Classes: 5
Class Labels
0: Background
1: Soil
2: Bedrock
3: Sand
4: Big Rock 

 Data Preparation


The first step involved identifying and removing outliers. Images having the same masks were
grouped and plotted. The ones that were completely
different (e.g. without Mars terrain) from the others
were considered outliers and so they were removed.
The dataset was split into training and validation
sets, maintaining the same class distribution. The
next step involved augmentation on training set
due to a lower presence of the label 4, representative
of the Big Rocks. The pipeline of this step involved
the use of Keras Albumentation library [2], characterized by the transformations such as RandomRotate, VerticalFlip, HorizontalFlip, ShiftScaleRotate, RandomBrightnessContrast. This library is
indicated for augmenting datasets for semantic segmentation, since it performs the transformations
both on the images and on the masks. In this way,
the number of images containing the label 4 was
increased by a factor of 100.


 Model design


The proposed model is the MultiResUnet, which
is an advanced implementation of the Unet that
includes in its architecture Multi-Resolution
Blocks and Residual Paths. In this case, the
model used is characterized by a depth of five levels,
each progressively capturing coarser global features
1
at deeper levels and finer semantic details at upper
ones.
The encoder consists of multiple levels, each with
MultiResBlocks that extract features at various
scales using 3x3, 5x5 and 7x7 convolutions.
These multi-scale features are combined through
concatenation and batch normalization to create a
robust representation, ensuring the acquisition of
information with different spatial resolution.
In the decoder, transposed convolutions are employed to increase spatial resolution, while features
from the corresponding encoder levels (via ResPath)
are integrated through concatenation. MultiResBlocks in the decoder ensure effective integration of
features for reconstructing the segmentation mask.
The model was compiled using Adam optimizer and
SparseCategoricalCrossentropy loss which is computed with this formula:
