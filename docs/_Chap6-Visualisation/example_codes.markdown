---
title: Matplotlib quick examples
author: Lei Lei
category: notes
layout: post
---

## Bar plots

```python
import matplotlib.pyplot as plt
import numpy as np

x = np.array(['ERS1', 'ERS2', 'ETR1', 'ETR2', 'EIN4'])
x_position = np.arange(len(x))

y_control = np.array([12.0, 3.1, 11.8, 2.9, 6.2])
y_stress = np.array([6.2, 3.4, 6.8, 2.0, 6.8])

fig = plt.figure()
ax = fig.add_subplot(1, 1, 1)
ax.bar(x_position, y_control, width=0.4, label='control')
ax.bar(x_position + 0.4, y_stress, width=0.4, label='stress')
ax.legend()
ax.set_xticks(x_position + 0.2)
ax.set_xticklabels(x)
plt.show()
```

### Stacked bar plot

```python
import matplotlib.pyplot as plt

labels = ['G1', 'G2', 'G3', 'G4', 'G5']
men_means = [20, 35, 30, 35, 27]
women_means = [25, 32, 34, 20, 25]
men_std = [2, 3, 4, 1, 2]
women_std = [3, 5, 2, 3, 3]
width = 0.35       # the width of the bars: can also be len(x) sequence

fig, ax = plt.subplots()

ax.bar(labels, men_means, width, yerr=men_std, label='Men')
ax.bar(labels, women_means, width, yerr=women_std, bottom=men_means,
       label='Women')

ax.set_ylabel('Scores')
ax.set_title('Scores by group and gender')
ax.legend()

plt.show()
```

### Box plot

```python
import numpy as np
import matplotlib.pyplot as plt

# Make a random dataset:
height = [3, 12, 5, 18, 45]
bars = ('A', 'B', 'C', 'D', 'E')
y_pos = np.arange(len(bars))

# Create bars
plt.bar(y_pos, height)

# Create names on the x-axis
plt.xticks(y_pos, bars)

# Show graph
plt.show()
```