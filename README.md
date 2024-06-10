# TML_Assignment2_Model_Stealing

- This assignment implements Model Stealing attack using the access to a victim encoder through API. The victim encoder is said to be protected by B4B. Based on this fact, we note that the representations returned  on querying the victim encoder are first applied different transformations and then returned to the attacker. This in turn reflects in the L2 distance obtained between the representations of the stolen and victim encoder.

## Data that attacker can access
- Partial training data of the victim model
- Information on the structure of the dataset like normalization parameters used.

## Approaches used
 - Our initial approach is as follows - Querying the images from the available partial training data to the victim encoder and obtaining the representations for 1000 images per query, storing them. After doing 100 queries (limit for an API), we stop querying and store all the obtained representations in a file. Next, we define a stolen encoder's architecture, which may/may not be similar to the victim encoder's architecture. We do that, since we note that for model stealing, it is not necessary to have the architecture of the stolen encoder similar to that of the victim encoder. We query the stolen encoder with the images from the training data, obtain the representations from the stolen encoder. Based on the obtained representations, we reduce the loss between the representaions obtained by querying the victim encoder and ones obtained by querying the stolen encoder. Then we see the stolen encoder replicating the behaviour of the victim encoder to some extent.
 - Our second approach leverages the fact that a victim encoder returns similar representations for augmented versions of the same image. We apply horizontal flipping and grayscale transformations to the available training data and create two augmented datasets from the original dataset. Further, we query the horizontally flipped augmented data to the victim encoder and obtain 100 representation files, which we store for further use. Next, we define a stolen encoder's architecture and query the grayscale augmented data to the stolen encoder. Like the first approach, we minimize the loss between the representaions obtained by querying the victim encoder and ones obtained by querying the stolen encoder.

## Results

## Further ideas on implementation

## Impact of B4B on the L2 distance between representations
