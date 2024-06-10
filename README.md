# TML Assignment2 Model Stealing in Self-Supervised Learning

- This assignment implements a Model Stealing attack using the access to a victim encoder through API. The victim encoder is said to be protected by B4B. Based on this fact, we note that the representations returned  on querying the victim encoder are first applied to different transformations and then returned to the attacker. This in turn negatively impacts the L2 distance obtained between the representations of the stolen and victim encoder.

## Data that the attacker can access
- Partial training data of the victim model
- Information on the structure of the dataset like normalization parameters used.

## Approaches used
 - Our first leverages the fact that a victim encoder returns similar representations for augmented versions of the same image. We apply horizontal flipping and grayscale transformations to the available training data and create two augmented datasets from the original dataset. Further, we query the horizontally flipped augmented data to the victim encoder and obtain 100 representation files, which we store for further use. Next, we define a stolen encoder's architecture and query the grayscale augmented data to the stolen encoder. Based on the obtained representations, we reduce the loss between the representations obtained by querying the victim encoder and those obtained by the stolen encoder. Then we see the stolen encoder replicating the behavior of the victim encoder to some extent.

 - Our second approach is as follows: querying the images from the available partial training data to the victim encoder, obtaining the representations for 1000 images per query, and storing them. After doing 100 queries (the limit for an API), we stop querying and store all the obtained representations in a file. Next, we define a stolen encoder's architecture, which may/may not be similar to the victim encoder's architecture. We do that since we note that for model stealing, it is unnecessary to have the architecture of the stolen encoder similar to that of the victim encoder. We query the stolen encoder with the images from the training data and obtain the representations from the stolen encoder. Like the first approach, we minimize the loss between the representations obtained by querying the victim encoder and those obtained by the stolen encoder.

## Results
| Approach                                  | L2 distance          |File implementing the approach  | 
|:-----------------------------------------:|:--------------------:|:-------------------------------|
| Augmentations and Mean Squared Error      | 11.316974639892578   | TML_Assignment2_Approach1.ipynb|
| No Augmentations and Mean Squared Error   | 11.385514259338379   | TML_Assignment2_Approach2.ipynb|
| Augmentations and Contrastive functions   | Data 3               | TML_Assignment2_Approach3.ipynb|
| NoAugmentations and Contrastive functions | Data 3               | TML_Assignment2_Approach4.ipynb|

## Further ideas on implementation

## Impact of B4B on the L2 distance between representations
