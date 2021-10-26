# Workshops
Docs and code samples from workshops, including software setup and PyTorch/TensorFlow tutorials

- [Software Setup](#software-setup)
  - [Integrated Development Environment (IDE)](#ide)
  - [Version Control](#version-control)
  - [Dependency Management and Virtual Environment](#dependency-venv)
  - [Installing PyTorch](#installing-pytorch)

# Software Setup <a name="software-setup"></a>

## Integrated Development Environment (IDE) <a name="ide"></a>

### Installing VSCode

## Version Control <a name="version-control"></a>

### Setting up Git on your local machine via SSH

### Cloning your project repository

## Dependency Management and Virtual Environment <a name="dependency-venv"></a>

### Creating a virtual environment with conda

### Creating a `requirements.txt` file and freezing dependencies

### Adding, commiting, and pushing changes with Git

## Installing PyTorch <a name="installing-pytorch"></a>

### Installation

PyTorch can be installed very easily using conda. Visit [PyTorch's website](https://pytorch.org) and select the configuration based on your machine. This will be `Stable` for your build, `Conda` for your package, `Python` for your language, and `CPU` for your compute platform. (Later on, when we run model training on GPU with CUDA, we'll install the appropriate CUDA versions, but don't worry about that for now!)

<img width="811" alt="image" src="https://user-images.githubusercontent.com/42232624/138920194-c443ef1c-88ac-4435-929c-3043a5408077.png">

Make sure your virtual environment is active in your terminal instance, and then run the `conda install ...` command that the PyTorch site has generated for you based on your configuration.
