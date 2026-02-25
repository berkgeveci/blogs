# Build & Run Notes for Each Language Example

## C++

- **Build system:** CMake with `find_package(VTK)`
- **VTK install:** Not tested in this session, but CMakeLists.txt exists at `examples/cpp/CMakeLists.txt`
- **VTK build/install:** Presumably uses the same VTK install or a separate C++ build

## Python

- **No build step needed**
- **Python:** `/Users/berk.geveci/Work/uv-venvs/3.13/bin/python`
- **VTK Python build:** `/usr/local/scratch/builds/vtk/py3.13`
- **PYTHONPATH:** `/usr/local/scratch/builds/vtk/py3.13/lib/python3.13/site-packages`
- **Run:**
  ```
  PYTHONPATH=/usr/local/scratch/builds/vtk/py3.13/lib/python3.13/site-packages \
    /Users/berk.geveci/Work/uv-venvs/3.13/bin/python examples/python/cone.py
  ```
- **Also works with:** `/usr/local/scratch/builds/vtk/py3.13/bin/vtkpython examples/python/cone.py`

## Java

- **Build system:** CMake with `UseJava` (no `find_package(VTK)` — JOGL dependency issue)
- **Environment module:** `module load openjdk`
- **VTK Java install:** `/usr/local/scratch/installs/vtk/java`
- **VTK JAR:** `/usr/local/scratch/installs/vtk/java/vtk-9.6.20260209.jar`
- **JNI native libs:** `/usr/local/scratch/installs/vtk/java/lib/java/vtk-Darwin-arm64/`
- **VTK native libs:** `/usr/local/scratch/installs/vtk/java/lib/` (found via rpath from JNI wrappers)
- **CMakeLists.txt requires:** `-DVTK_JAR=... -DVTK_NATIVE_DIR=...`
- **Configure & build:**
  ```
  cmake -S examples/java -B build \
    -DVTK_JAR=/usr/local/scratch/installs/vtk/java/vtk-9.6.20260209.jar \
    -DVTK_NATIVE_DIR=/usr/local/scratch/installs/vtk/java/lib/java/vtk-Darwin-arm64
  cmake --build build
  ```
- **Run:** `./build/run.sh` (requires `module load openjdk`)
- **Notes:**
  - `find_package(VTK)` doesn't work because the VTK install requires JOGL (not installed, not needed for this example)
  - The CMakeLists.txt generates a `run.sh` that uses `-XstartOnFirstThread` (required on macOS for GUI)
  - The run script uses `-Djava.library.path` pointing to the JNI wrappers directory only; rpath (`@loader_path/../../`) in the JNI dylibs resolves back to the VTK native libs

## C#

- **Build system:** dotnet SDK (.csproj file)
- **Environment module:** `module load dotnet`
- **dotnet SDK:** `~/.dotnet` (dotnet 10.0)
- **NuGet package:** `Kitware.VTK` version `9.5.2025.1212` (from nuget.org, cached in `~/.nuget/packages/`)
- **ActiViz distribution:** `/Users/berk.geveci/Work/dotnet/ActiViz.NET-9.5.2025.1212-Darwin-arm64-Supported/`
- **Created:** `examples/csharp/cone.csproj`
- **Build & run:**
  ```
  cd examples/csharp
  dotnet run
  ```
- **Clean up:** Remove `bin/` and `obj/` directories after testing

## F#

- **Build system:** dotnet SDK (.fsproj file)
- **Environment module:** `module load dotnet`
- **NuGet package:** `Kitware.VTK` version `9.5.2025.1212` (same as C#)
- **Created:** `examples/fsharp/cone.fsproj`
- **Note:** F# requires explicit `<Compile Include="cone.fsx" />` in the .fsproj (unlike C# which auto-includes .cs files)
- **Build & run:**
  ```
  cd examples/fsharp
  dotnet run
  ```
- **Clean up:** Remove `bin/` and `obj/` directories after testing

## JavaScript (VTK.wasm)

- **No build step needed** — runs in browser via script tag
- **Created:** `examples/javascript/cone.html`
- **Updated:** `examples/javascript/cone.js` (rewritten from vtk.js to VTK.wasm)
- **Dependencies loaded from CDN:**
  - UMD bundle: `https://unpkg.com/@kitware/vtk-wasm/vtk-umd.js`
  - WASM tarball: `https://gitlab.kitware.com/api/v4/projects/13/packages/generic/vtk-wasm32-emscripten/9.5.20251215/vtk-9.5.20251215-wasm32-emscripten.tar.gz`
- **Run:** Serve over HTTP (browsers won't load WASM from `file://`):
  ```
  python3 -m http.server 8080 --directory examples/javascript
  open http://localhost:8080/cone.html
  ```
- **Notes:**
  - This is the real VTK C++ compiled to WebAssembly, not vtk.js (which is a JS reimplementation)
  - All VTK method calls are async (`await`) due to the JS–WASM boundary
  - Method names use camelCase (`setResolution`, `getOutputPort`)
  - RenderWindow and Interactor take a `canvasSelector` to bind to an HTML canvas
  - Needed `await renderer.resetCamera()` for proper initial view
  - Reference repo cloned to: `/Users/berk.geveci/Work/VTK/vtk-wasm`

## Julia

- **No build step needed**
- **Julia:** `/opt/homebrew/bin/julia` (version 1.12.3)
- **Installed:** `PyCall` package (`Pkg.add("PyCall")`) — installs to `~/.julia/`
- **PyCall configured to use:** `/Users/berk.geveci/Work/uv-venvs/3.13/bin/python`
  - Set with: `ENV["PYTHON"]="/Users/berk.geveci/Work/uv-venvs/3.13/bin/python"; Pkg.build("PyCall")`
- **VTK Python build:** `/usr/local/scratch/builds/vtk/py3.13`
- **Run:**
  ```
  PYTHONPATH=/usr/local/scratch/builds/vtk/py3.13/lib/python3.13/site-packages \
    julia examples/julia/cone.jl
  ```

## Ruby

- **No build step needed**
- **Environment module:** `module load ruby`
- **Ruby module updated:** Changed path from `/opt/homebrew/Cellar/ruby/3.3.0/bin/` to `/opt/homebrew/Cellar/ruby/4.0.1/bin/` (Homebrew had upgraded)
- **Ruby:** version 4.0.1
- **Installed:** `pycall` gem (`gem install pycall`) — version 1.5.2
- **Python for pycall:** Set via `PYTHON` env var at runtime
- **VTK Python build:** `/usr/local/scratch/builds/vtk/py3.13`
- **Run:**
  ```
  module load ruby
  PYTHON=/Users/berk.geveci/Work/uv-venvs/3.13/bin/python \
  PYTHONPATH=/usr/local/scratch/builds/vtk/py3.13/lib/python3.13/site-packages \
    ruby examples/ruby/cone.rb
  ```
- **Code fix:** `pyimport` with dotted module names requires `as:` alias in newer pycall:
  ```ruby
  pyimport 'vtkmodules.vtkRenderingOpenGL2', as: :vtkRenderingOpenGL2
  pyimport 'vtkmodules.vtkInteractionStyle', as: :vtkInteractionStyle
  ```

## Clojure

- **No build step needed**
- **Clojure:** `/opt/homebrew/bin/clojure` (version 1.12.4)
- **Environment module:** `module load openjdk` (needs Java)
- **VTK Java install:** Same as Java example
- **Run:**
  ```
  module load openjdk
  clojure \
    -Sdeps '{:deps {} :paths ["." "/usr/local/scratch/installs/vtk/java/vtk-9.6.20260209.jar"]}' \
    -J-XstartOnFirstThread \
    -J-Djava.library.path=/usr/local/scratch/installs/vtk/java/lib/java/vtk-Darwin-arm64 \
    -M -i examples/clojure/cone.clj
  ```
- **Notes:**
  - First run downloads Clojure dependencies from Maven Central
  - Warning about external paths in `:paths` is a deprecation notice (harmless)

## Groovy

- **No build step needed**
- **Installed:** Groovy via Homebrew (`brew install groovy`) — version 5.0.4
- **Environment module:** `module load openjdk` (needs Java)
- **VTK Java install:** Same as Java example
- **Run:**
  ```
  module load openjdk
  JAVA_OPTS="-XstartOnFirstThread -Djava.library.path=/usr/local/scratch/installs/vtk/java/lib/java/vtk-Darwin-arm64" \
    groovy --classpath /usr/local/scratch/installs/vtk/java/vtk-9.6.20260209.jar \
    examples/groovy/cone.groovy
  ```
- **Code fix:** Wildcard `import vtk.*` doesn't work in Groovy 5 — changed to explicit imports:
  ```groovy
  import vtk.vtkNativeLibrary
  import vtk.vtkConeSource
  ...
  ```
- **Notes:**
  - `-DstartOnFirstThread` doesn't work as a groovy flag (wrong format); must use `JAVA_OPTS`
  - `-Scp` overrides the entire classpath (removes Clojure's classes); use `--classpath` instead
