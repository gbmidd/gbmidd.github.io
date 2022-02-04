---
layout: single
title:  "Fine Tuning a Transformer Model"
excerpt: "Use your own data to update a model"
date:   2022-01-10 17:31:15 -0700
categories: howto
read_time: true
---

Suppose you have some training data you'd like to use to update a language model for a specific task: in this case, I'd like to "teach" the model that two passages of text are semantically similar, to help me with similarity search.

We'll use the (wonderful) `sentence-transformers` library, and I'll assume you have some training data of the form

```python
data = [['sentence 0', 'very similar to sentence 0'],
        ['sentence 1', 'very similar to sentence 1'],
        ...
       ]
```

Import necessary libraries:

```python
from numpy.random import permutation, random

from sentence_transformers import SentenceTransformer, SentencesDataset, InputExample
from sentence_transformers import evaluation, losses, util

from torch.utils.data import DataLoader
```

Our first step will be to process the training data to get it ready for the model.  Specifically, we need to assigne a "score" for how similar our sentence pairs are.  If you have some human-generated score, this is preferable -- if not, we can randomly assign one as shown below.  Here I model the scores as a Gaussian distribution about 0.9, with a variance of 0.05.

```python
mean = 0.9
scaling = 20.0
scores = mean + (random(len(data)) - 0.5) / scaling
```

In the absence of better scoring, this is a reasonable substitute.  Reducing the mean or widening the variance will give the model less confidence in the similarity between the language, and may avoid overfitting.  Now, let's build data structures suitable for the trainer, and split the data set into training and evaluation.  I first permute the data so that if I do this again, I have a different split of training / evaluation.

```python
fract_train = 0.8

pairs = [InputExample(texts=d, label=scores[ix]) for ix, d in enumerate(data)]
perm = permutation(len(pairs))

num_train_pairs = int(len(pairs) * fract_train)
num_eval_pairs = int(len(pairs) * (1 - fract_train))

train_pairs = [pairs[perm[ix]] for ix in range(num_train_pairs)]
eval_pairs = [pairs[perm[ix]] for ix in range(num_train_pairs, num_train_pairs + num_eval_pairs)]
```

Now we're ready to set up our data loader:

```python
train_batch_size = 32

train_dataset = SentencesDataset(train_pairs, model)
train_dataloader = DataLoader(train_dataset, shuffle=True, batch_size=train_batch_size)
```

Now, we define our model and corresponding loss function.

```python
model = SentenceTransformer('all-MiniLM-L6-v2',
                            cache_folder='./')

train_loss = losses.MultipleNegativesRankingLoss(model=model,
                                                 similarity_fct=util.dot_score,
                                                 scale=1)
```

We're ready to fit the model:

```python
warmup_steps = 100
num_epochs = 8

model.fit(train_objectives=[(train_dataloader, train_loss)],
          epochs=num_epochs,
          warmup_steps=warmup_steps,
          output_path='./new-model/')
```
