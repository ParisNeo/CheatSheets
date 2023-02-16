# Conda Cheat Sheet

 
# Install conda
Download and install Anaconda or Miniconda from the official website (https://www.anaconda.com/products/individual) based on your operating system.

# Create and manage environments

## Create a new environment
```
conda create --name myenv
```
## Create a new environment with a specific python version
```
conda create --name myenv python=3.10
```


## Activate an environment
```
conda activate myenv
```
## Deactivate an environment
```
conda deactivate
```
## List all environments
```
conda env list
```
## Clone an environment
```
conda create --name myclone --clone myenv
```

## Remove an environment
```
conda remove --name myenv --all
```

# Install packages

## Search for a package
```
conda search package_name
```

## Install a package
```
conda install package_name
```

## Install a specific version of a package
```
conda install package_name=1.0
```
## Install multiple packages
```
conda install package1 package2
```

## Remove a package
```
conda remove package_name
```

# Manage channels

## Add a channel
```
conda config --add channels channel_name
```
## Remove a channel
```
conda config --remove channels channel_name
```

## List channels
```
conda config --get channels
```

# Update conda and packages

## Update conda
```
conda update conda
```

## Update all packages in the current environment
```
conda update --all
```

# Export and import environments

## Export an environment to a file
```
conda env export > environment.yml
```
## Create an environment from a file
```
conda env create -f environment.yml
```

# Miscellaneous commands

## Check version
```
conda --version
```

## View the list of installed packages
```
conda list
```

## Check dependencies of a package
```
conda info package_name
```

## Check for updates of installed packages
```
conda search --outdated
``` 
