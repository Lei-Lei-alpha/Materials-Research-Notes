---
title: Pytorch tricks
author: Lei Lei
category: notes
layout: post
---

## Tensors

## Functions

### Constraint output

~~~python
def forward(self,x):
    x = self.main(x)
    output = x.clamp(-5,5).torch.sigmoid()*scale
    return output
~~~

## Models

## Loss functions

## Activation functions
