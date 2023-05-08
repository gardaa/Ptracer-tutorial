# Installing Conda, Creating a 'ptracer' Environment, and Activating it on Ubuntu (Optional)

Before installing ptracer we should create an environemt, this step is not mandatory but it is recommended. \
Here there are step-by-step instructions on how to install Conda, create an environment named 'ptracer', and activate it on an Ubuntu system.

## Prerequisites

- Ubuntu operating system (preferably 18.04 or later)

## Installation Steps

### 1. Update Ubuntu

Before installing Conda, it's a good practice to update your Ubuntu system packages:

```bash
sudo apt update && sudo apt upgrade
```{{exec}}

### 2. Install Conda
Download the latest version of Miniconda installer for Linux from the [official website](https://docs.conda.io/en/latest/miniconda.html).

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```{{exec}}
Change the downloaded script file to an executable file:
```bash
chmod +x Miniconda3-latest-Linux-x86_64.sh
```{{exec}}
Run the installation script:

```bash
./Miniconda3-latest-Linux-x86_64.sh
```{{exec}}
Follow the prompts during the installation process. \
When the installation is complete update the configuration file to apply the changes.
```bash
source ~/.bashrc
```{{exec}}
### 3. Create the 'ptracer' Environment
To create a new Conda environment named 'ptracer', run the following command:


```bash
conda create --name ptracer
```{{exec}}
### 4. Activate the 'ptracer' Environment
Activate the 'ptracer' environment by running:

```bash
conda activate ptracer
Now, you are inside the 'ptracer' environment. You can install packages and run your project inside this environment.
```{{exec}}
