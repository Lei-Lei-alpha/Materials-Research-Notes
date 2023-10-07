---
layout: post
title: Committee neural network potentials
author: Lei Lei
category: Lab Notes
---

## Concept

Instead of a single model, multiple models are trained independently to form a committee that offers:

- Averaging over the predictions improves accuracy
- Disagreement between the committee (measured by the standard deviation of the predictions of the members), provides access to an estimate of the generalisation error
- Substantially reduce overfitting issues
- Active learning strategy known as query by committee (QbC): adding previously unlabeled data with maximal committee disagreement to the training set to systematically improve the model

