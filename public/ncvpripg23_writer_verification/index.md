# Winning the NCVPRIPG23 Challenge on Writer Verification

<!--more-->
## The Challenge
The statement is simple. Given two images of handwritten texts, predict whether they've been written by the same person or not. This challenge was a part of the [8th National Conference on Computer Vision, Pattern Recognition, Image Processing and Graphics](https://vl2g.github.io/challenges/wv2023). You can go and have a look at the above link for the details of the challenge and dataset (as of writing the dataset isn't public).  

{{< image src="/images/ncvpripg23_wv.png" caption="Writer Verification Challenge" height="500" width="500" linked="false" >}}

You can also have a look at the [project repository](https://github.com/cs-mshah/NCVPRIPG2023_Writer_Verification) along with this blog.


{{< admonition type="abstract" title="TL;DR" open=false >}}
- Used a `ResNet50` with `DINO` pretrained weights.
- Trained the network using triplet loss.
- Final test AUC is **0.97588** in just 10 epochs.
{{< /admonition >}}

## Exploratory Data Analysis (EDA)
For exploring computer vision datasets I use [Voxel51](https://docs.voxel51.com/), which is one of the coolest tools to qualitatively view your dataset. You can go ahead and read their extensively detailed documentation along with tutorials to get an idea of the power it has. Using it for this challenge was as simple as doing

`pip install fiftyone`

```python
import fiftyone as fo

def fiftyone_vis(dataset_dir: str):
    # Create the dataset
    dataset = fo.Dataset.from_dir(
        dataset_dir=dataset_dir,
        dataset_type=fo.types.ImageClassificationDirectoryTree,
        name=name,
    )
    session = fo.launch_app(dataset)
    session.wait()
```

The above code is generic enough to view any dataset organized in the standard [:`ImageFolder`](https://pytorch.org/vision/stable/generated/torchvision.datasets.ImageFolder.html#imagefolder) structure of `PyTorch`. The `session.wait()` is important to keep the server running. Here is the amazing visualization which we get:

{{< image src="/images/voxel51.png" caption="voxel51" height="500" linked="false" >}}

We can clearly see the class labels and several other filters/tools in the dashboard. The images are of varying sizes and with only a few images per writer class. The written text contains English alphabets, special symbols (punctuations, superscript, digits, special chars), lines, paragraphs, 2 text boxes in one image, printed text, strikes, written text going outside the black boundary box, text written at angles and a few blank texts.  

Although there is a lot of scope for preprocessing, I started without it to find out the initial results, which turned out to be pretty decent.
## Model Architecture
Due to a large number of writer classes and only a few images per class, the problem can be modelled into a **metric learning task**, where the goal is to learn a similarity function between pairs of images. With this similarity metric, we would finally be able to predict whether two texts come from the same writer (by setting an appropriate threshold).  

{{< admonition type="note" title="Evaluation Metric" open=true >}}
As the problem is that of binary classification, the metric for the challenge is the [AUC score](https://analyticsindiamag.com/complete-guide-to-understanding-roc-curves/).
{{< /admonition >}}

The overall architecture consists of two things
- The encoder part (backbone) to encode images.
- The loss function to train the model on.

### The Image Encoder
The encoder is a simple `ResNet50` with an output dimension of `64` (instead of the default 1000). For getting a good backbone, I thought of using [DINO](https://github.com/facebookresearch/dino) pretrained weights on ImageNet. Several papers have shown this backbone to be strong in downstream metric learning tasks. I just started with a `ResNet50`, but a `ViT` could've been tried too, along with the more recent [DINOv2](https://github.com/facebookresearch/dinov2) weights. I used a generic classifier from the [Transfer Learning Library](https://github.com/thuml/Transfer-Learning-Library/blob/master/tllib/modules/classifier.py), which can be configured to have different backbones, bottlenecks and heads. Another benefit is that the *learning rates* of different layers *can also be configured*.

### The Triplet Loss
The triplet loss seemed to be one of the ideal choices to train the network on. The motivation comes from the following:

{{< image src="https://miro.medium.com/v2/resize:fit:828/format:webp/1*ADu4F-SoI-cLqyO0IqqBrQ.png" caption="[Triplet Loss - Advanced Intro](https://towardsdatascience.com/triplet-loss-advanced-intro-49a07b7d8905)" linked="false" >}}

The loss is defined by,

{{< admonition type="note" title="Loss Function" open=true >}}
{{< math >}}
$$
    L(a,p,n) = max\\{d(a_i, p_i)-d(a_i, n_i) + margin, 0\\}
$$
{{< /math >}}
Where,  
$a - \textnormal{denotes the anchor}$  
$p - \textnormal{denotes the positive (image from the same writer class as anchor)}$  
$n - \textnormal{denotes the negative (image from a different writer class as anchor)}$  
$d - \textnormal{distance metric (CosineSimilarty in this case)}$  
$\textnormal{margin }-\textnormal{ distance between (ap) and (an) pairs}$
{{< /admonition >}}

{{< admonition type="tip" title="pytorch-metric-learning" open=true >}}
For implementing losses, miners and samplers, I used the state of the art [pytorch-metric-learning](https://github.com/KevinMusgrave/pytorch-metric-learning) library.
{{< /admonition >}}

Triplets are mined by a $\verb|TripletMarginMiner|$ function depending on [specific conditions](https://kevinmusgrave.github.io/pytorch-metric-learning/miners/\#tripletmarginminer). I kept the margin as **0.2** (could've been a hyperparameter) in all experiments.

### Training

A learning rate of **0.01** was used for the head *(fc layer)*, **0.001** was used for the backbone, along with *adam* as the optimizer and an exponentially decaying $lr$ scheduler.  
The $lr$ decays every epoch according to,

$$lr = lr_0(1 + 0.001lr)^{-0.75}$$
The choice of hyperparameters in this case is purely based on some past experience with using the `Transfer-Learning-Library`.

{{< admonition type="note" title="MPerClassSampler" open=true >}}
Instead of the standard [`RandomSampler`](https://pytorch.org/docs/stable/data.html#torch.utils.data.RandomSampler) for sampling images of a batch, I made use of an [:`MPerClassSampler`](https://kevinmusgrave.github.io/pytorch-metric-learning/samplers/#mperclasssampler) from the `pytorch-metric-learning` library, which samples $M=4$ images of every class in a batch. The benefit of doing this is a **much** faster convergence due to the production of a large number of triplets in a batch, making it more useful than random sampling.
{{< /admonition >}}

{{< admonition type="success" title="weights and biases logs" open=true >}}
The entire training logs of all experiments can be found at: [wandb](https://wandb.ai/manan-shah/NCVPRIPG23-Writer-Verification?workspace=user-manan-shah)
{{< /admonition >}}

{{< admonition type="success" title="Training plots of the best model" open=false >}}
![](https://i.imgur.com/6fEOyWh.png)
{{< /admonition >}}

## Results and Conclusions
Here is an overview of some of the ablations that were conducted.

|AUC(Val) |Batch Size|Miner|Sampler|Epochs|
|:--|:--|:--|:--|:--|
|0.7217|32|semihard|-|20|
|0.9642|128|semihard|-|80|
|0.9782|128|all|-|80|
|0.9775 |128|all|MPerClass|80|

- It is clear from the table that one needs a larger batch size (which is obvious in metric learning tasks).
- `all` mining strategy generates more triplets, and this strategy works slightly better than the other options.
- An `MPerClassSampler` greatly improves training time.
- The AUC of the best model on the `val` set turned out to be **0.9775** and that of the test set turned out to be **0.97588**

Overall, it was an amazing experience for me to **win** my **first** challenge.
