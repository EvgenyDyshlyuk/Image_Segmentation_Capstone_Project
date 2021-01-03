# Image Segmentation CoreAI Bootcamp Capstone Project
## VOCSegmentation 2007 - 2012 dataset is used

Fully convolutional U-Net architechture is selected for this task. Fully-convolutional implies that it doesn't contain fully-connected layers, but only convolutional, max-pooling, and batch normalization layers all of which are invariant to the size of the image. This allows the network to be able to accept images of any dimension. Practically upsampling should be done on the image of even diminsion - so for UNET this implies image size multiple of 2^3 = 8, and for ResNetUnet image size mulitple 2^5 =32
Convolutional neural nets are not scale-invariant. For example, if one trains on the cats of the same size in pixels on images of a fixed resolution, the net would fail on images of smaller or larger sizes of cats. In order to overcome this problem, I know of two methods (might be more in the literature):
- multi-scale training of images of different sizes in fully-convolutional nets in order to make the model more robust to changes in scale (used here with image augmentation with random resizing)
- having multi-scale architecture 
sourse: (https://ai.stackexchange.com/questions/6274/how-can-i-deal-with-images-of-variable-dimensions-when-doing-image-segmentation)

Although different image sizes are possible, practically while training I did not find the way to put images of different size to batches. Pytorch custome collate_fn approach does not seem to allow training in batches - looks same as training on batches of single image. Single image batch training was too slow and I changed to another approach:
- train and val images are randomly cropped to 224X224 size
- train images are in addition augmented using random: resize, rotation, horizontal flip and color jitter
- test images are only trimmed to proper size so that image size is multiple to 8 (Unet) or 32 (ResnetUnet)

## Training
- I utilized all avalible credits on Google Colab and training seems to flatten out. 
- ResNetUnet showed better results(expected) and I used it. Pretrained version of ResNet showed better convergence than the not pretrained one (expected as well).
- I trained the model using a combination of Dice and binary cross entropy (BCE) loss - this loss showed some improvements. 
Loss functions (https://medium.com/@junma11/loss-functions-for-medical-image-segmentation-a-taxonomy-cefa5292eec0)