# animplotlib

IMPORTANT: CURRENTLY NOT WORKING AND UNDERGOING FIXES - DO NOT USE AFTER fa062c7 COMMIT

This package acts as a thin wrapper around the
`matplotlib.animation.FuncAnimation` class to simplify animating `matplotlib`
plots.

![](examples/gifs/trefoil-knot.gif)

## Installation

```
pip install animplotlib
```


## User manual  

There are two classes which can be called: `AnimPlot`, for 2-D plots,
and `AnimPlot3D`, for 3-D plots.

### AnimPlot

As an example, below is a demonstration of the steps required to make a
basic plot of an Euler spiral. An Euler spiral can be obtained by plotting
the [Fresnel integrals](https://en.wikipedia.org/wiki/Fresnel_integral),
which can be generated using `scipy.special`.

Import the necessary libraries and create a matplotlib figure and axes:

```python
import animplotlib as anim
import numpy as np
import matplotlib.pyplot
import scipy.special as sc

fig = plt.figure()
ax = fig.add_subplot(111)
```

Generate the points being plotted:

```python
x = np.linspace(-10, 10, 2500)
y, z = sc.fresnel(x)
```

Create an empty `matplotlib` plot and set x and y limits:

```python
lines, = ax.plot([], [], lw=1)

ax.set_xlim(-1, 1)
ax.set_ylim(-1, 1)
```
 
Call the `AnimPlot` class:

```python
anim.AnimPlot(fig, lines, y, z)
plt.show()
```

Optional arguments:
* `trail` (`bool`) :  set to `True` by default. If `False` only the 'ith'
  point is plotted each frame.
* `plot_speed` (`int`) : set to 10 by default.
* `save_as` (`str`) : file name to save the animation as a gif in the
  current working directory.
* `**kwargs` : other arguments passable into
`matplotlib.animation.FuncAnimation` (see [the docs](https://matplotlib.org/3.1.0/api/_as_gen/matplotlib.animation.FuncAnimation.html) for more info).

![](examples/gifs/fresnel_2d.gif)

### AnimPlot3D

Creating a 3-D animated plot is similar to creating a 2-D plot but with a
few additions.

```python
import animplotlib as anim
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D  # for 3-D matplotlib plots
import scipy.special as sc

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

x = np.linspace(-10, 10, 3000)
y, z = sc.fresnel(x)
```

For 3-D plots, two empty `matplotlib`, each contained within a list, must
be created:

```python
lines, = [ax.plot([], [], [])]
points, = [ax.plot([], [], [], 'o')]
```

The second plot, `points`, by default plots the 'ith' point each frame.

```python
ax.set_xlim(-10, 10)
ax.set_ylim(-1, 1)
ax.set_zlim(-1, 1)

# make the axes look a bit cleaner
ax.xaxis.pane.fill = False
ax.yaxis.pane.fill = False
ax.zaxis.pane.fill = False

ax.xaxis.pane.set_edgecolor('w')
ax.yaxis.pane.set_edgecolor('w')
ax.zaxis.pane.set_edgecolor('w')

ax.grid(False)

anim.AnimPlot3D(fig, ax, lines, points, x, y, z, plot_speed=5)
plt.show()
```

![](examples/gifs/fresnel_3d.gif)


Optional arguments:
* `plot_speed` (`int`) : set to 10 by default.
* `rotation_speed` (`int`) : proportional to `plot_speed`. Off by default,
enabled by setting a value.
* `l_num` (`int`) : The number of points being plotted to `lines` each frame. By
default, all the points up until the current point get plotted.
* `p_num` (`int`) : The number of points being plotted to `points` each frame. By
default, this is set to 1, i.e. only the current most point is plotted each
frame (the orange point in the gif).
* `save_as` (`str`) : file name to save the animation as a gif in the
  current working directory.
* `**kwargs` : other arguments passable into
`matplotlib.animation.FuncAnimation` (see [the
docs](https://matplotlib.org/3.1.0/api/_as_gen/matplotlib.animation.FuncAnimation.html)
for more info).
