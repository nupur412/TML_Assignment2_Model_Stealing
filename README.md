# TML Assignment 2 
## Model Stealing in Self-Supervised Learning

This assignment implements a Model Stealing attack using the access to a victim encoder through API. The victim encoder is said to be protected by B4B. Based on this fact, we note that the representations returned  on querying the victim encoder are first applied to different transformations and then returned to the attacker. We make use of this information for our approach. The stolen encoder, model in onnx format submitted for evaluation, and all representations can be found [here](https://bit.ly/tml_assignment2_team3)

## Data accessible to the adversary
- Partial training data of the victim model.
- Information on the structure of the dataset.
- API access to the victim encoder.
- The victim encoder is protected using B4B defense.

## Approach used
The implemented approach can be accessed in the [**TML_Assignment2_Final_Approach.ipynb**](https://github.com/nupur412/TML_Assignment2_Model_Stealing/blob/main/TML_Assignment2_Final_Approach.ipynb) file. Bucks for Buckets (B4B) is an active defense strategy used to protect the victim encoder. As per B4B defense, when we query images to the victim encoder, transformations like Affine, Pad + shuffle, Affine + Pad + Shuffle, and Binary are applied to the actual image representations. We leverage this fact and also consider that augmented versions of an image when queried to the victim encoder return similar representations. 
Hence, we create 2 augmented datasets by applying horizontal flipping and grayscale transformations to the original dataset. When we query the victim encoder with the augmented dataset1 (horizontal flipping), we find that different representations are returned when we query the same image multiple times to the victim encoder. We hypothesize that this occurs due to the transformations applied to the actual representations before their return. We consider this observation and query the same set of 1000 images to the victim encoder 7 times and obtain 7 different representations for an image. We observe that for some images, the elements in the 7 representations are slightly different, whereas, for some images, the representations obtained vary significantly. We wish to obtain accurate victim representations, hence we find the average/mean of all 7 representations obtained for every image in the provided dataset to get closer to the actual victim representations. Further, we define a model architecture for the stolen encoder. This model architecture is independent of the victim model's architecture and can be used even when it is not similar to the victim encoder's architecture. To train the stolen encoder, we make use of the augmented dataset2 (grayscale), obtain representations by querying the stolen encoder, map them to the victim representations obtained previously, and then try minimizing the loss between the victim and stolen encoder's representations using Mean Squared Error (MSE) loss function. We use MSE because it is used to directly minimize distances between representations from the victim and the stolen copy.

### Why only 7 queries per image?
The query limit per API is 100. The length of the provided dataset is 13,000. In one query, we can obtain the representations for 1000 images. Thus, we need 13 queries in total to obtain representations for 13000 images. However, based on the previous discussion in the approach section, we understand that the representations returned for an image vary when queried multiple times. Thus, to obtain a variety of representations for an image, we decide to query the victim encoder with an image x number of times. This number x is calculated based on the query limit we have been given - 100/13 ~ 7 queries per set, meaning 7 queries per image. This helps us to make complete use of the available number of queries while also obtaining a wide variety of representations of an image.

## Results
The above approach results in an L2 distance of 10.007421493530273 for 30% of the private data.
On using InfoNCE loss contrastive loss function, we obtain an L2 distance of around 30, which can possibly be reduced after hyperparameter tuning, which we did not try.

## Other ideas on implementation that our approach misses
- Using other stolen model architectures can help reduce the L2 distance.
- Using contrastive loss functions to reduce the loss between stolen and victim encoder's representations.
- Using no augmentations for the provided dataset
