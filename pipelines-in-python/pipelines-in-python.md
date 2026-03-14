
In my [last blog](https://www.kitware.com/vtk-trame-jupyter-magic/),
I used this syntax in one example:

```python
cone >> mapper
```

This is one of the many shortcuts we have been introducing to make writing Python code and scripts easier. It is equivalent to:

```python
mapper.SetInputConnection(cone.GetOutputPort())
```

If you would like to know more about VTK pipelines, I refer you to the [VTK book](https://book.vtk.org/en/latest/VTKBook/04Chapter4.html). Here, I will focus on this new syntax (introduced in VTK 9.4 by the way).

## Basics

Let's start building slightly more complicated pipelines.

Without the `>>` syntax:

```python
from vtkmodules import vtkFiltersSources as sources
from vtkmodules import vtkFiltersGeneral as filters_general
from vtkmodules import vtkRenderingCore as rendering_core

cone_source = sources.vtkConeSource()

shrink = filters_general.vtkShrinkPolyData()
shrink.SetInputConnection(cone_source.GetOutputPort())

mapper = rendering_core.vtkPolyDataMapper()
mapper.SetInputConnection(shrink.GetOutputPort())
```

With it:

```python
cone_source = sources.vtkConeSource()
shrink = filters_general.vtkShrinkPolyData()
mapper = rendering_core.vtkPolyDataMapper()
cone_source >> shrink >> mapper
```

or even (if you don't need to manipulate `cone_source` or `shrink` later):

```python
mapper = rendering_core.vtkPolyDataMapper()
sources.vtkConeSource() >> filters_general.vtkShrinkPolyData() >> mapper
```

In addition to saving us from typing a few extra characters, this notation has the benefit of representing the pipeline visually in one line, making the code easier to follow. You can represent fairly complicated pipelines visually this way:

```python
cone_source >> shrink >> mapper1
cone_source >> transform >> mapper2
```

## Multiple connections

Multiple input connections are supported:

```python
append = filters_core.vtkAppendPolyData()
(sources.vtkConeSource(), sources.vtkSphereSource()) >> append
```

or

```python
append = filters_core.vtkAppendPolyData()
sources.vtkConeSource() >> append
sources.vtkSphereSource() >> append

# to clean the inputs to append do this:
[] >> append
```

## Multiple ports

You might be wondering how multiple port filters are handled. Take `vtkClipPolyData`. It
splits a polygonal dataset into two parts: one on the positive side of an [implicit
function](https://book.vtk.org/en/latest/VTKBook/06Chapter6.html#implicit-functions),
the other on the negative side. So the filter has two output ports. Here is an example
pipeline:

```python
from vtkmodules import vtkCommonDataModel as data_model

mapper0 = rendering_core.vtkPolyDataMapper()
mapper1 = rendering_core.vtkPolyDataMapper()

clip = filters_core.vtkClipPolyData()
clip.SetImplicitFunction(data_model.vtkPlane())

mapper0.SetInputConnection(clip.GetOutputPort(0))
mapper1.SetInputConnection(clip.GetOutputPort(1))
```

In order to handle cases like this, we introduced the `select_ports` function:

```python
from vtkmodules.util.execution_model import select_ports

select_ports(clip, 0) >> mapper0
select_ports(clip, 1) >> mapper1
```

You can also use it to select input ports. For example, the `vtkStreamTracer` filters
expects two input ports: the vector dataset (port 0), the seed dataset (port 1).

```python
stream_tracer = filters_flowpaths.vtkStreamTracer()
plot3d_data >> select_ports(0, stream_tracer)
line_seeds >> select_ports(1, stream_tracer)
```

Note that the syntax is asymmetric. For output ports: `select_ports(filter, port)`, for input
ports: `select_ports(port, filter)`. If there is a crazy filter out there that has multiple
input ports and multiple output ports, we got you covered: `select_ports(input_port, filter, output_port)` (please don't try this at home).

## Pipeline objects

One more trick up our sleeves. The `>>` operator returns a proper pipeline
object which can be used to create what I would call pipeline operators.
For example:

```python
shrink = filters_general.vtkShrinkPolyData()

clip = filters_core.vtkClipPolyData()
clip.SetImplicitFunction(data_model.vtkPlane())

# This is now a reusable shrink + clip pipeline
pipeline = shrink >> clip

cone_source = sources.vtkConeSource()
cone_source.Update()
cone = cone_source.GetOutput()

shrinked_clipped_cone = pipeline(cone)

sphere_source = sources.vtkSphereSource()
sphere_source.Update()
sphere = sphere_source.GetOutput()

shrinked_clipped_sphere = pipeline(sphere)
```

I admit that this example was a bit too verbose. Let's make it
simpler as a teaser for a future post.

```python
shrink = filters_general.vtkShrinkPolyData()

clip = filters_core.vtkClipPolyData()
clip.SetImplicitFunction(data_model.vtkPlane())

# This is now a reusable shrink + clip pipeline
pipeline = shrink >> clip

shrinked_clipped_cone = pipeline(sources.vtkConeSource()())
shrinked_clipped_sphere = pipeline(sources.vtkSphereSource()())
```

## Imagine

What do you think?

```python
from vtkmodules import express as vtk_express

shrink = filters_general.vtkShrinkPolyData()
clip = filters_core.vtkClipPolyData(
    implicit_function = data_model.vtkPlane())
# This is now a reusable shrink + clip pipeline
pipeline = shrink >> clip

shrinked_clipped_cone = pipeline(vtk_express.make_cone())
shrinked_clipped_sphere = pipeline(vtk_express.make_sphere())
```

Or for those that hate pipelines (you know who you are), this?

```python
i_plane = vtk_express.make_implicit_plane()

shrinked_clipped_cone = (
    vtk_express.make_cone().shrink().clip(implicit_function = i_plane))

shrinked_clipped_sphere = (
    vtk_express.make_sphere().shrink().clip(implicit_function = i_plane))
```

