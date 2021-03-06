# MeshTools

[![releases](http://img.shields.io/github/release/c3m-labs/MeshTools.svg)](http://github.com/c3m-labs/MeshTools/releases)
[![SemVer 2.0.0](http://img.shields.io/badge/SemVer-2.0.0-brightgreen.svg)](http://semver.org/spec/v2.0.0.html)

_MeshTools_ is a [Mathematica](http://www.wolfram.com/mathematica/) package for creating and manipulating
meshes for finite element method (`ElementMesh` objects). It helps you to do the following tasks:

* Create structured mesh in 2D or 3D
* Split, transform and merge meshes
* Convert triangular to quadrilateral mesh
* Create high quality mesh on basic geometric shapes

![niceExample](Images/ExampleMeshes.png)

## Installation

_MeshTools_ package requires Mathematica version 11. or later.

The package is released in the `.paclet` file format that contains the code,
documentation and other necessary resources.
Download the latest `.paclet` file from the
repository ["releases"](https://github.com/c3m-labs/MeshTools/releases) page
to your computer and install it by evaluating the following command in the Mathematica:

```mathematica
(* This built-in package is usually loaded automatically at kernel startup. *)
Needs["PacletManager`"]

(* Path to .paclet file downloaded from repository "releases" page. *)
PacletInstall["full/path/to/MeshTools-X.Y.Z.paclet"]
```

This will permanently install the _MeshTools_ package to `$UserBasePacletsDirectory`.
To update the documentation it may be necessary to restart Mathematica.
Mathematica will always use the latest installed version of package and all installed versions
can be enumerated by evaluating `PacletFind["MeshTools"]`.
You can get more detailed information about the package with `PacletInformation["MeshTools"]`.
All versions can be uninstalled with:

```mathematica
PacletUninstall["MeshTools"]
```

Alternative method of installation for Mathematica version 12.0+ is to use
[ResourceFunction["GitHubInstall"]](https://resources.wolframcloud.com/FunctionRepository/resources/GitHubInstall)
from Wolfram Function Repository. Note that you need to be logged in with (free) Wolfram Account to use it.

```mathematica
ResourceFunction["GitHubInstall"]["c3m-labs", "MeshTools"]
```

### Try it online

You can try the package even without installation by going to
[Wolfram Programming Lab](https://www.wolfram.com/programming-lab/)
and clicking "Start programming now".
This will open a new notebook in Cloud environment where you should evaluate the following to load the package.

```mathematica
Get["http://raw.githubusercontent.com/c3m-labs/MeshTools/master/MeshTools/MeshTools.wl"]
```

You will not be able to access documentation, but it is helpful to list all available functions.
Then you can also try the usage example from the section below.

```mathematica
Information["MeshTools`*"]
```

## Usage

After installing the package, load it in the Mathematica session with `Needs`.
To access the documentation, open the notebook interface help viewer and search for "MeshTools".

### Example of extruded mesh

```mathematica
Needs["MeshTools`"]

(* Create MeshRegion object from Graphics. *)
region = DiscretizeGraphics[
  Text[Style["\[Pi]", FontWeight -> "Bold"]],
  _Text,
  MaxCellMeasure -> 1/10
]

(* Convert MeshRegion object to ElementMesh object and smoothen mesh (improve quality). *)
meshTri = SmoothenMesh@ToElementMesh[region, "MeshOrder" -> 1]
meshTri["Wireframe"]
```

![screenshot1](Images/PiMeshTriangle.png )

```mathematica
(* Convert triangular mesh to quadrilateral and smoothen it again. *)
meshQuad = SmoothenMesh@TriangleToQuadMesh@meshTri
meshQuad["Wireframe"]
```

![screenshot2](Images/PiMeshQuad.png )

```mathematica
(* Extrude 2D quadrilateral mesh to 3D hexahedral mesh (with 8 layers). *)
mesh3D = ExtrudeMesh[meshQuad, 1, 8];
mesh3D["Wireframe"["MeshElementStyle" -> FaceForm@LightBlue]]
```

![screenshot3](Images/PiMesh3D.png )

```mathematica
(* Inspect the minimal, average and maximal quality of 3D mesh. *)
Through[{Min, Mean, Max}@Flatten[mesh3D["Quality"]]]
(* {0.47, 0.91, 0.99} *)
```

## Contributing and feedback

Please use the repository ["issues"](https://github.com/c3m-labs/MeshTools/issues) page to submit bugs or feature ideas.
If you find this package useful, feel free to send us feedback by email to `github(at)c3m.si`.

Pull requests to this repository are welcome.
For major changes, please open an issue first to discuss what you would like to change.
Instructions on building the `.paclet` file from source code can be found in [CONTRIBUTING.md]( CONTRIBUTING.md ) file.

## License

[MIT](https://choosealicense.com/licenses/mit/)
