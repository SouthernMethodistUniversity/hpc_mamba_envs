# HPC Mamba Environments

The goal of this repository is to create a versioned and scripted method for creating stackable Mamba environments that can be loaded in an HPC environment.

## Desired outcomes

1. Reproducible environments based on `json` config files
2. Scoped environments so users can load packages most useful to them without additional bloat
3. Reduced reliance on *per user* created environments which has caused needless and excessive duplication of packages on high performance storage

## Basic design principles

1. Base environments `json` files are created without versions, when possible
2. When building environmnents, versions are specified as necessary and otherwise try to use the latest compatible version
   1. Dated and versioned `yml` files are saved when an environment is built with the exact packages used
   2. Dated and versioned `lua` module files are generated to load the environment
   3. All `json`, `yml` and `lua` files and any associated scripts are tracked in Github
       1. Environments that trigger security alerts may be depricated and archived
3. Environments are nested, e.g. access to environments built with `Python 3.11` first requires loading the module associated with `python/3.11`
4. Environments should have unit tests that can be run on the installed system.
   1. At minumun verify `import package_name` works for all specified packages in environment
   2. Verify dependencies are correctly loaded, e.g. Cuda enabled packages can see a GPU (if present)
   3. Job scripts to submit unit tests 
