
# A (hopefully) helpful guide for model compression



Models are trained by data science teams but putting them in real-world production software, especially edge applications is hard.

Software teams have to manage energy consumption, latency issues, disk size limitations, RAM size limitations, parallelism with machine learning.

I'm going to walk you through my research on these topics to save you some time. 


# General Solutions for Model Compression

## Model Quantization

In many cases, especially with LLMs, you can convert your weights from floating to int8 types. This reduces disk size required to store your model (float 32 require 32 bits, int8s require 8 bits) on disk. 

Quantization also helps with RAM / VRAM. In particular, since models operate with massive linear matrix multiplications, if we can reduce the size of the matrices being multiplied, we reduce the RAM/VRAM required to operate on those matrices.

After you've quantized your model, be sure to evaluate your performance as this approach is lossy.

You can use beta PyTorch software <a href="https://pytorch.org/docs/main/quantization.html" target="_blank">quantization docs</a> to help you with this if you're working with a PyTorch model (or to generally learn more).

## Model Pruning

## Knowledge Distillation

You can retrain a smaller version of your model architecture to achieve the same performance as your large version of your model: just use your larger version of the model as a teacher and your small version of the model as the learner and learn from teacher. This should be seen as a last resort, after you've compiled and quantized your model as it's much more involved. 

## Advanced Research techniques / resources

See Qualcomm's research team's [Aimet on GitHub](https://github.com/quic/aimet), that walks you through their implementation of some of these methods and discusses other options (including Singular Value Decomposition of weight matrices).


## Considerations

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
## Disk Size

Problem: Machine Models take up a lot of hard disk space. This makes cloud computing easy but edge computing hard. For applications where data security is key (health, military, etc.), edge computing is an absolute necessity so making sure no data escapes.

## RAM

...

### About Me

I'm Sam Randall. I've worked as an iOS Engineer embedding ML applications and I've worked to develop data-based tools for data scientists to improve their models' performance. I try and keep abreast of the latest trends in software engineering, machine learning, mathematics and their intersections. Model Compression in Edge Software is very high on that list.