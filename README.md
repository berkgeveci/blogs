# VTK Blog Posts

A collection of blog posts about the Visualization Toolkit (VTK), covering
Python integration, pipeline architecture, streaming, zero-copy data
structures, and more.

These posts were originally published on
[berkgeveci.github.io](https://berkgeveci.github.io) between 2014 and 2015.

## Blog Posts

| Date | Post | Topic |
|---|---|---|
| 2014-07-28 | [Improved VTK - numpy integration](vtk-python/vtk-python.md) | numpy_interface module introduction |
| 2014-07-31 | [VTK - numpy integration (part 2)](vtk-python-2/vtk-python-2.md) | Composite datasets and arrays |
| 2014-08-07 | [VTK - numpy integration (part 3)](vtk-python-3/vtk-python-3.md) | Algorithms and NumPy |
| 2014-08-12 | [mpi4py and VTK](mpi4py/mpi4py.md) | MPI with VTK and NumPy |
| 2014-08-18 | [VTK - numpy integration (part 4)](vtk-python-4/vtk-python-4.md) | More algorithms |
| 2014-08-21 | [Sending VTK data objects](send-dataobjects/send-dataobjects.md) | MPI data transfer |
| 2014-08-25 | [VTK - numpy integration (part 5)](vtk-python-5/vtk-python-5.md) | Advanced topics |
| 2014-08-27 | [Plans for VTK](vtk-plans/vtk-plans.md) | VTK development roadmap |
| 2014-09-02 | [vtkProgrammableFilter](programmable-filter/programmable-filter.md) | Programmable filter usage |
| 2014-09-05 | [vtkPythonAlgorithm](vtk-python-algorithm/vtk-python-algorithm.md) | Writing VTK algorithms in Python |
| 2014-09-13 | [Developing HDF5 readers](h5py-reader/h5py-reader.md) | HDF5 readers with h5py |
| 2014-09-18 | [A VTK pipeline primer (part 1)](pipeline/pipeline.md) | Pipeline basics |
| 2014-10-04 | [VTK issue hackathon](issue-hackathon/issue-hackathon.md) | Community event |
| 2014-10-05 | [A VTK pipeline primer (part 2)](pipeline-2/pipeline-2.md) | RequestInformation pass |
| 2014-10-26 | [A VTK pipeline primer (part 3)](pipeline-3/pipeline-3.md) | RequestUpdateExtent and RequestData |
| 2014-11-09 | [Streaming in VTK: Time](streaming-time/streaming-time.md) | Temporal streaming |
| 2014-11-16 | [A simple particle advection example](simple-particle/simple-particle.md) | Particle integration |
| 2014-11-26 | [Streaming in VTK: Space](streaming-space/streaming-space.md) | Spatial streaming |
| 2014-11-28 | [Spatial streaming and compositing](compositing/compositing.md) | Compositing and parallel rendering |
| 2015-01-08 | [HDF5 writer and reader](h5py-writer-reader/h5py-writer-reader.md) | Temporal HDF5 I/O |
| 2015-01-19 | [Block streaming](block-streaming/block-streaming.md) | Block-based streaming |
| 2015-02-12 | [Zero copy arrays](zero-copy-arrays/zero-copy-arrays.md) | Custom data array layouts |
| 2015-02-13 | [Zero copy: synchronized templates](zero-copy-sync-templates/zero-copy-sync-templates.md) | Updating filters for mapped arrays |
| 2015-02-20 | [Cell set as unstructured grid](cell-set/cell-set.md) | Implicit connectivity |
| 2015-03-07 | [Threshold with cell set](threshold/threshold.md) | Efficient threshold filter |
| 2025-02-24 | [VTK: The polyglot toolkit](vtk-the-polyglot/vtk-the-polyglot.md) | VTK across 10 programming languages |
| 2026-02-25 | [VTK + Tcl/Tk: The Return](vtk-tcl-tk/vtk-tcl-tk.md) | Reviving VTK's Tcl/Tk bindings via Python interop |

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
