# ML Wiki- FAQ, Resources and Reference
>Some quirks, resources and not so obvious things about ML models. A FAQ style document.
>This is not a comprehensive wiki. It does however, capture some salient features and minute quirks about certain concepts that summaries and short explanations tend to skip.        
>I intend to include use cases for each concept and important references.
>For personal use for the most part.

# QUESTIONS

#### Why do Resnet (and later)training curves see step-wise improvments in accuracy after seemingly coverging ?
The learning rate is stepped by a factor of 10 every few 1000 iterations. Everytime the LR is decreased, we see a large improvment in accuracy.

#### Why do feature maps misalign in CNNs and why is it a problem for semantic seg?
The performance of a semantic segmentation model depends on accurate superimposition of predicted segmentation maps and ground truth.      
Superimposition implies that the final features maps should be aligned perfectly with the ground truths. In this context,   it would require the center and scale of the final segmentation map to be identical to ground truths and input feature maps.

However, certain downsampling operations, namely max pooling and strided convolutions will translate the center of the downsampled output feature map with respect to the input feature map. (Note: Padding method for downsampling = 'SAME', computed as D_out = Ceil(D_in/Stride) )
This misalignment caused by translation is unavoidable for certain combinations of input feature map and filter size.
The combination are as follows: (for 'SAME' paddingMode, and Stride =2 )     
Here SAME implies padding chosen such that OutputDImension = Ceiling( inputDimension / Stride )

##### Common combinations and their effects on filterSize 

| Stride  | padding |kernel  | Input | Result
| ------------- | ------------- |------------- | ------------- | ------------- |
| 2  | SAME  |  odd| odd |No misalignment | 
| 2  | SAME  |  even | odd |center misaligned | 
| 2  | SAME  |  odd | even |center misaligned | 
| 2  | SAME  |  even | even |no misalignment |
| 1  | NONE  | odd | odd |No misalignment (dimension decreases by 1) |

# DEFINITIONS AND JARGON

### Merge / Fuse layers 
2 feature maps of identical dimensions are added in an elementwise fashion.         
Sizes of all input layers and out layer are identical.      

### Add layers
Might mean merge or contatenate. Depends on author.Still confusing at time. Needs updating ?

### Residual connection 
A type of skip connection that is explicitly used in residual blocks.       
Has 2 channels in 1 block. Both channels diverge at input, and converge by merging/ fusing at the end of the block.      
1st channel is a standard set of conv-BN-Relu operations.      
The 2nd channel applies a 1x1 convolution to match the number of channels for merging. The 1x1 conv. can be strided to reduce spatial dimension to match 1st channel's downsampling.     
*why?* :    
avoiding signal degradtion during backprop through very deep NNs      
Identity mappings in deep resnets have a hypercolumns like effect where the net can better use intermediate features.[ref](https://arxiv.org/abs/1603.05027) 

### 1x1 convolution (Pointwise convolution)
A 1x1xK kernel, that operates on one pixel column in a DxDxK feature map.     
Serves to decrease the number of channels while preserving spatial dimensions.

### Attention
By and large, Attention(QW<sup>j</sup> , KW<sup>i</sup>, V<sup>i</sup>) is the importance of the j<sup>th</sup> representation of the query to the i<sup>th</sup> representation of the key that relates to the i<sup>th</sup> value that must be transformed.         
The importance between 2 vectors can be computed in different ways. The original transformer uses a scaled dot product.

# Models explained 

### Transformer - Multi head attention and encoder with no directionality
Encoder-decoder style multi head attention model for sequence transduction

### BERT - Denoising autoencoder transformer
Add bidirectionality to transformers by using a masked language model pretraining process. It includes masking part of the sentence and then trying to complete4 the task using the context. Masked pretraining is done on sentrnce reconstruction and next sentence predictions tasks.

# RESOURCES

### Receptive Field Computation
[[*ref*]](https://stackoverflow.com/questions/35582521/how-to-calculate-receptive-field-size)![](https://raw.githubusercontent.com/AnishPimpley/ML-wiki/master/CodeCogsEqn.png)

### OutputSize of Convolutional Operation
![](https://raw.githubusercontent.com/AnishPimpley/ML-wiki/master/png.png)

### Padding size to  preserve input dimensions (for stride 1)
0.5 * (filterSize -1 ) * DilationRate

### Must have visualization tool for Neural networks:
[Netscope](http://ethereon.github.io/netscope/quickstart.html) : Takes caffe prototxt files and gives detailed visualizations even the more complex models.

### inline Latex editor
http://latex.codecogs.com/eqneditor/editor.php

### inline image creator
https://yuml.me/diagram/scruffy/class/samples

# PAPER SUMMARIES

* Relation Networks for Object Detection - Hu et al. CVPR 2018 - [link](https://github.com/AnishPimpley/Personal-ML-FAQ/blob/master/Summaries/RelationNetworksForObjectDetection.md)     

* Deep extraction of manga structural lines - Li et al. ACM TOG 2017 - [Presentaion](https://docs.google.com/presentation/d/1tgEAshcduu6F-ZtREg-hK7pIOyhsnmWOhnF78dtFQg0/edit?usp=sharing)

* Effects of valence and arousal on working memory performance in VR games - Gabana et al. ACII 2017 - [Presentation](https://docs.google.com/presentation/d/12me2wJxNHPe08m9V_zlM6tTTwZnhSPro0NPwmn7VMmA/edit?usp=sharing)
