# Game project hierarchy

This section will describes the hierarchy of game projects.

Xenko uses a **package** system, where each game is itself contained in a package, and can also use content from some other packages. This allows to share resources across multiple games.

A package is a container for the following content:

- Game assets
- Code libraries
- A set of dependencies, ie. a reference to another package that allows to use the content of this package.

A package can be just a simple container of the above elements, or a game. When the package is a game, it also contains code executables (one per platform).

Packages are saved on the disk with the *.xkpkg* extension.

# Game assets

[Assets](../glossary.md#asset) are the data, meshes, textures, materials, sounds,... used in your game. Most of these assets are generated while importing a source file, such as an audio file or a 3D model while some other can be created from scratch (like entities). For each asset, a single file is created, usually with an extension starting by *.xk*. For instance, texture uses the *.xktex* extension. Assets can be sorted into folders and sub-folders for easy browsing. The root path of the assets of a package is stored in the package file.

# Code libraries

A package can contain one or multiple code libraries. A code library is a C# project that generates a single assembly. The package file references directly the associated Visual Studio project file (.csproj). If the package is a game package (and not just a resource package), it should also contain code executable. There is one executable per targeted platform. Basically these executables are simple platform-dependent entry points to the game code that is cross-platform.

# Dependencies

A package can reference each other to get access to the assets they contain. By default, each package has a reference to the Xenko package. It is a special package that contains the engine shaders and other mandatory data.

# Solution

Xenko uses Visual Studio solution files to list all the packages and code project related to a game. In this way, the Xenko Game Studio and Visual Studio can easily be integrated to gather ; they use the same root file for your project. By default, the Game Studio creates a new solution file when you make a new project, managing references to both C# projects and packages.

