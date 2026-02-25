# VTK Blog Posts

A collection of blog posts about the Visualization Toolkit (VTK), covering
Python integration, pipeline architecture, streaming, zero-copy data
structures, and more.

These posts were originally published on
[berkgeveci.github.io](https://berkgeveci.github.io) between 2014 and 2015.

## Blog Posts

| Post | Topic |
|---|---|
| [Improved VTK - numpy integration](vtk-python/vtk-python.md) | numpy_interface module introduction |
| [VTK - numpy integration (part 2)](vtk-python-2/vtk-python-2.md) | Composite datasets and arrays |
| [VTK - numpy integration (part 3)](vtk-python-3/vtk-python-3.md) | Algorithms and NumPy |
| [mpi4py and VTK](mpi4py/mpi4py.md) | MPI with VTK and NumPy |
| [VTK - numpy integration (part 4)](vtk-python-4/vtk-python-4.md) | More algorithms |
| [Sending VTK data objects](send-dataobjects/send-dataobjects.md) | MPI data transfer |
| [VTK - numpy integration (part 5)](vtk-python-5/vtk-python-5.md) | Advanced topics |
| [Plans for VTK](vtk-plans/vtk-plans.md) | VTK development roadmap |
| [vtkProgrammableFilter](programmable-filter/programmable-filter.md) | Programmable filter usage |
| [vtkPythonAlgorithm](vtk-python-algorithm/vtk-python-algorithm.md) | Writing VTK algorithms in Python |
| [Developing HDF5 readers](h5py-reader/h5py-reader.md) | HDF5 readers with h5py |
| [A VTK pipeline primer (part 1)](pipeline/pipeline.md) | Pipeline basics |
| [VTK issue hackathon](issue-hackathon/issue-hackathon.md) | Community event |
| [A VTK pipeline primer (part 2)](pipeline-2/pipeline-2.md) | RequestInformation pass |
| [A VTK pipeline primer (part 3)](pipeline-3/pipeline-3.md) | RequestUpdateExtent and RequestData |
| [Streaming in VTK: Time](streaming-time/streaming-time.md) | Temporal streaming |
| [A simple particle advection example](simple-particle/simple-particle.md) | Particle integration |
| [Streaming in VTK: Space](streaming-space/streaming-space.md) | Spatial streaming |
| [Spatial streaming and compositing](compositing/compositing.md) | Compositing and parallel rendering |
| [HDF5 writer and reader](h5py-writer-reader/h5py-writer-reader.md) | Temporal HDF5 I/O |
| [Block streaming](block-streaming/block-streaming.md) | Block-based streaming |
| [Zero copy arrays](zero-copy-arrays/zero-copy-arrays.md) | Custom data array layouts |
| [Zero copy: synchronized templates](zero-copy-sync-templates/zero-copy-sync-templates.md) | Updating filters for mapped arrays |
| [Cell set as unstructured grid](cell-set/cell-set.md) | Implicit connectivity |
| [Threshold with cell set](threshold/threshold.md) | Efficient threshold filter |
| [VTK: The polyglot toolkit](vtk-the-polyglot/vtk-the-polyglot.md) | VTK across 10 programming languages |

## Structure

Each blog post lives in its own self-contained directory:

```
blog-name/
  blog-name.md      # The blog post
  images/            # Referenced images (if any)
  examples/          # Code examples (if any)
```

## License

Prose is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
Code is licensed under the [BSD 3-Clause License](LICENSE.md).
Copyright (c) 2014-2015, Kitware, Inc.
