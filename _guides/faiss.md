---
layout: single
title:  "Quantized Index using FAISS"
excerpt: "Using a GPU to accelerate computation"
date:   2022-01-10 17:31:15 -0700
categories: howto
read_time: true
---

For anyone doing a nearest-neighbor search in a vector space, scaling can quickly become a problem.  Facebook has open-sourced a solution they use for this problem, [FAISS](https://github.com/facebookresearch/faiss).

Let's see how we can build a large index and quantize it for fast lookup, using a GPU.  If you haven't already, go ahead and install faiss with gpu support: `poetry install faiss-gpu`.

Import necessary libraries:

```python
import numpy as np
import faiss
```

First, we will create a large number of vectors for quantization.  Mirroring a project I did recently, I'll use vectors of length 300.  Let's create 5 million of them:

```python
dim = 300
vectors = np.random.rand(5e6, dim)
```

Now let's create our index object.  FAISS offers several metrics on which to search, I'll use minimum L2 distance.  Our next step will be to create an "Inverted File Product-Quantization" index from our flat (full precision) index.  The IVFPQ index will allow us to
 - quantize each dimension
 - define Voronoi cells to reduce complexity

```python
nlist = 10000			# Number of Voronoi cells to create
bits = 8 			# Precision of numerical representation
sub_quantizers = 20		# Number of subquantizers to use

index_flat = faiss.IndexFlatL2(dim)
index = faiss.IndexIVFPQ(index_flat, dim, nlist, sub_quantizers, bits)
```

From here, we could add our vectors and build the index.  However, using only a CPU, this would be expensive: the "training" step is where FAISS quantizes each vector and defines the Voronoi cells.  It's an iterative process that, for large indices, is best suited for a GPU.

```python
res = faiss.StandardGpuResources()

# Move the index onto the GPU memory
gpu_index = faiss.index_cpu_to_gpu(res, 0, index)

# Structure index, then add vectors
gpu_index.train(vectors)
gpu_index.add(vectors)
```

Now that we have our index built, let's move it back to a CPU for lookup -- in the event that the machine hosting the index doesn't have a GPU.  We'll also build a map so that we can reconstruct elements of the index.  Let's also save it.

```python
cpu_index = faiss.index_gpu_to_cpu(gpu_index)
cpu_index.make_direct_map()

faiss.write_index(cpu_index, 'index.ivfpq')
```

Using this approach, we've been able to reduce the on-disk size of an index from 5 GB to around 150 MB -- without an appreciable decline in performance!
