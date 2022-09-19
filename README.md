# Houdini Packages

## Overview

Traditionally, the way to set the values of environment variables in Houdini was to edit the `houdini.env` file. The most important variable to set up is the `$HOUDINI_PATH`, which tells Houdini where to look for files such as assets. You can also create convenience variables (for example `$TEXTURE_DIR`) and edit system variables (such as the binary search path `$PATH`).

The problem is that not only the user wants to set environment variables, but also studios, plug-in authors, and so on. Multiple parties would end up rewriting or appending to the `houdini.env` file, sometimes causing errors or redundancy.

Also, you might want variables defined some times and not others (for example, switching between project-specific paths, or turning “experimental” assets on and off).

The new solution for this kind of set-up is _packages_. Packages are `.json` files in `HOUDINI_PATH/packages`. Each package file contains a specification for how to modify the environment (including the Houdini Path and other variables).

You can then have separate packages for:

*   Studio variables.
    
*   Project variables.
    
*   User variables.
    
*   Variables for each separate plug-in, tool, or add-on.
    

The file format supports a small expression language to allow some automation. With expressions you can, for example:

*   Enable/disable a tool installation or specify what package(s) are required by a tool.
    
*   Write generic package files.
    
*   Write a single package to manage a multi-platform setup.
    
*   Set variables conditionally with simple operators, Houdini keywords and environment variables.
    

This removes the need to edit `houdini.env`.

## Using package files

*   Package files must be valid JSON and must have the `.json` extension.
    
*   Houdini will scan the following directories on startup (if they exist) for package files:
    
    *   `$HOUDINI_USER_PREF_DIR/packages`
        
    *   `$HSITE/houdinimajor.minor/packages` (for example, `$HSITE/houdini18.0/packages`)
        
    *   `$HOUDINI_PACKAGE_DIR`
        
        Houdini will process the package files directly in the folders specified in `HOUDINI_PACKAGE_DIR`, _do not_ add a `packages` directory under these folders.
        
        `$HSITE` must be set to an existing directory before starting Houdini.
        
    *   `$HFS/packages`
        
    
*   Houdini process files it finds first in the directory order above, then within each directory alphabetically by file name. The processing order can be changed with the process_order keyword.
    
*   You can have multiple package files in each `packages` directory on the above path.
    
*   Houdini does _not_ scan subdirectories inside `packages`.
    
*   Extra package folders can be specified dynamically from a package file.
    

## Writing the package file


The following keywords are used for writing a package:

`env`

Sets or modifies environment variables.

`enable`

Enables or disables a package.

`load_package_once`

Prevents a package from being loaded multiple times. Houdini will load the first package in the search path with `"load_package_once": true`, and ignore any other packages with the **same file name** in the search path. This keyword defaults to `false`.

`process_order`

Specifies an integer value that Houdini can use for arranging the order of the packages to process in a package directory.

`package_path`

Scans package paths dynamically.

`path`

Shortcut to set `HOUDINI_PATH`.

`recommends`

Checks specific package(s) availability without enforcing a dependency.

`requires`

Checks specific package(s) availability and enforces a dependency.

# Futher Reading
https://andcgi.medium.com/houdini-packages-multiple-render-engines-environment-variable-c517df426440
