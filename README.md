A short guide to managing conda/anaconda/miniconda @ CRL and on Research Computing e2 cluster

**NOTE: full guide TBC - guide for e2 conda is below**

# Installing independent conda environment on e2

This is a short guide to installation of your own conda environment on e2 (BCH Research Computing cluster). 

TLDR (too long didn't read)
- conventional way to create conda environment: `conda create -n newenv python=3.6`
- workaround on e2: `mkdir -p ~/conda_pkgs/; conda create -p ~/conda_pkgs/newenv python=3.6`

Why do we need "the workaround"?  
e2 conda is hosted in this location `/programs/local/anaconda/`. By design, this location is not writable by e2 users without sudo access. Therefore, adding / updating / removing conda packages is not possible. Instead, we install conda into our own local directory as a workaround. 

**Note**  
This workaround does NOT require installation of new conda software. We use preinstalled conda binaries on e2 cluser to create new conda environments in local directories. If you want to start from complete scratch, we recommend that you use [miniconda install files.](https://docs.conda.io/en/latest/miniconda.html) 

## Basic setup 
Login to e2 from your CRL machine  
`ssh $USER@e2.tch.harvard.edu`

Create a folder where your conda environment will be stored.  
`mkdir -p ~/conda_pkgs/`  
<sub>Please choose the location carefully. If you keep adding new conda environments to this folder, it will quickly fill up and possibly take up many Gb in size.</sub>

Activate conda on e2 first  
`module load anaconda3` 

List available environments (supplied by research computing): 
`conda env list` 

## Create a basic environment 
e.g. imagine you need to create a new conda environment wiht python 3.6 and nifti & nrrd file readers 

`conda create --prefix ~/conda_pkgs/my_custom_env/ python=3.6 pynrrd nibabel -c kayarre -c conda-forge`

## Create an environment using a pre-requisite environment file 
In this example we use a .yaml file from our DCE MRI reconstruction github repository, that specifies all the necessary python dependencies. [https://github.com/azizkocana/dce_mri](https://github.com/azizkocana/dce_mri).   
<sub>Note that this is a private repository - if you do not have access to it with your github account you must ask the owner (Aziz) to add you. </sub> 

#### Step 1: Clone repository.  

`git clone https://github.com/azizkocana/dce_mri.git`

<sub> Note that we only need to perform this step as we are interested in python dependencies file (`requirements.yml`), which we will use directly to create our conda environment. You can also download a .yaml file manually. </sub>

#### Step 2: Create new conda environment from .yml file 
`conda env update --file ~/dce_mri/requirements.yml --prefix ~/conda_pkgs/dce_mri_env/`

#### Step 3: Activate environment 
`source activate /home/ch215616/conda_pkgs/dce_mri_env`  
<sub> Note that `source activate` is equivalent to `conda activate`. On e2 servers you are best to use `source activate` since there is some incompatibilities that cause `conda activate` to break on e2 servers only. </sub>

#### Step 4: Verify 
Verify if correct environment is installed and activated (look out for '*' next to activated env)  
`conda env list`  
Verify installed packages  
`conda list`

## Clone an existing environment on e2 cluster 
E.g. this may be useful if you want to start from an already existing working environment on e2 with many dependencies (which were not installed by you) but need to install additional packages. Since it is not possible to modify e2 conda environments by design, we can instead clone the e2 conda environment locally into our own directory and then add/remove packages inside the clone. 

#### Step 1: activate existing environment that you are interested in. 
e.g. tensorflow-gpu conda environment `tf-gpu`   
`source activate tf-gpu `

#### Step 2: Clone this environment to your local folder 
`conda create --clone tf-gpu --prefix ~/conda_pkgs/tf-gpu-myclone/`

####  Step 3: Make modifications to the clone 
`source activate ~/conda_pkgs/tf-gpu-myclone/`   
`conda install -c conda-forge tensorflow-probability`

## Create a new environment and install dependencies via `pip`
This may be useful if you are planning to use someone else's software whose dependencies are different from yours. In this case, we will create a new conda environment, and then use pip (python package manager) to install python dependencies supplied by the code author. 

e.g. we will use the Noise2Noise library by Nvidia Labs 

#### Step 1: Create a new basic environment 
`conda create --prefix ~/conda_pkgs/noise2noise/ python=3.6`  
`source activate ~/conda_pkgs/noise2noise/`

#### Step 2: Install dependencies via pip 
`git clone https://github.com/NVlabs/noise2noise`   
`pip install -r noise2noise/requirements.txt`




