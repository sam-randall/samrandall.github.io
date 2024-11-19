
# A (hopefully) helpful guide for model compression

Models are trained by data science teams but putting them in real-world production software, especially edge applications is hard.

Software teams have to manage energy consumption, latency issues, disk size limitations, RAM size limitations, parallelism with machine learning.

I'm going to summarize my research on these topics to save you some time. 

*Right now, it's articulated in a way that's helpful for software engineers interested in deploying ML models and it's not especially focused on engaging the research community (no research works cited).*


# General Solutions for Model Compression

## Model Quantization

In many cases, especially with LLMs, you can convert your weights from floating to int8 types. This reduces disk size required to store your model (float 32 require 32 bits, int8s require 8 bits) on disk. 

Quantization also helps with RAM / VRAM (VRAM means Video RAM which is the GPU equivalent of RAM). Conceptually, both refer to the amount of memory space required by your model to make a computation. In particular, since models operate with massive linear matrix multiplications, if we can reduce the size of the matrices being multiplied, we reduce the RAM / VRAM required to operate on those matrices.

After you've quantized your model, be sure to evaluate your performance as this approach is lossy.

You can use beta PyTorch software <a href="https://pytorch.org/docs/main/quantization.html" target="_blank">quantization docs</a> to help you with this if you're working with a PyTorch model (or to generally learn more).

You can also do this in [Tensorflow](https://www.tensorflow.org/model_optimization/guide/quantization/post_training). 

There's another concept here called Quantization-Aware Training. This is what it sounds like: you create a model architecture during training that can be easily scaled down after training. Talk to your data science teams to hear if they're doing that. See [Pytorch on Quantization Aware Training](https://pytorch.org/blog/quantization-aware-training/).

## Model Pruning

I don't currently know much about model pruning but this seems to be a helpful guide [Model Pruning Medium Article](https://towardsdatascience.com/how-to-prune-neural-networks-with-pytorch-ebef60316b91).

Generally, my understanding is that by zeroing out the weights you can convert dense matrices into sparse onces while achieving roughly the same performance, by *discarding values from your weight matrices nearly equal to zero*.

## Knowledge Distillation

You can retrain a smaller version of your model architecture to achieve the same performance as your large version of your model: just use your larger version of the model as a teacher and your small version of the model as the learner and learn from teacher. This should be seen as a last resort, after you've compiled and quantized your model as it's much more involved. 
Here's a study on the SOTA of Knowledge Distillation in the space of LLMs:
[Knowledge Distillation Survey Paper](https://arxiv.org/abs/2402.13116).

Here's another blog on the subject. 
[Knowledge Distillation](https://medium.com/@nminhquang380/knowledge-distillation-explained-model-compression-49517b039429)

## Advanced Research techniques / resources

See Qualcomm's research team's [Aimet on GitHub](https://github.com/quic/aimet), that walks you through their implementation of some of these methods and discusses other options (including Singular Value Decomposition of weight matrices).

## Glossary

1. Loss-less compression
2. Lossy compression

In the ideal state of loss-less compression, you achieve greater compression while accepting a hopefully slight 

Sometimes you're OK with a loss in accuracy (or other performance metric) in exchange for gains in model size or speed. In these cases, you lose fidelity (i.e lossy compression). 

## Latency

Problem: For real time applications, latency of models is crucial to get right. For applications like self-driving cars, this means the project will fail to reach production or for lower stakes productions, it will diminish the user experience.

Problem-Specific Solutions:
1. Pytorch Compilation 
(<a href="https://pytorch.org/docs/stable/generated/torch.compile.html" target="_blank">link</a>)
    
Like with C, you can compile your Pytorch code. According to Pytorch <a href="https://pytorch.org/tutorials/recipes/regional_compilation.html" target="_blank">docs</a>, `torch.compile` "effectively inlines [your model layers], producing a large [computation] graph to compile". 

## RAM

VRAM means Video RAM which is the GPU equivalent of RAM. Conceptually, both refer to the amount of memory space required by your model to make a computation.

### About Me

I'm Sam Randall. I've worked as an iOS Engineer embedding ML applications and I've worked to develop data-based tools for data scientists to improve their models' performance. I try and keep abreast of the latest trends in software engineering, machine learning, mathematics and their intersections. Model Compression in Edge Software is very high on that list.

If you want to talk about how to compress your models, let's talk. I'm eager to take on consulting work. 