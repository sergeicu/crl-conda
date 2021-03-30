A quick guide to managing conda/anaconda/miniconda @ CRL network and Research Computing e2 cluster

THIS GUIDE IS NOT COMPLETE. BELOW ARE DRAFT COMMENTS. 

# How does conda work 

# Difference between anaconda, conda and miniconda 

# Difference between pip and conda 

# 


# Installing conda 

## Install without root access 

## Install via root access 



# Installing conda on e2

## Copy an existing e2 environment 
module load anaconda3
source activate tf-gpu
conda create --name tf-gpu-myclone --clone tf-gpu --prefix /home/ch215616/myconda/

## Copy your own e2 environment from home folder 

## Copy dependencies from github (via conda)

## Copy dependencies from github (via pip)

## Copy dependencies from github (setup tools version)

# Conda and storage space 




Firstly, I use 'clone'  to avoid the trouble of handling my own installation of GPU & cuda dependencies (since ResearchComputing had already worked it out for me) 
Secondly, I use '--prefix' to tell conda where to create this new environment relates files ('envs') . That is the alternative way to bypass the lack of 'write' access to '/programs/local/anaconda/2019.10/3/envs/​'

You can also tell conda to create ALL future envs in a custom location by appending the following to ~/.condarc config file: 

channels:
  - conda-forge
  - defaults
pkgs_dirs:
  - /home/ch215616/myconda/pkgs/
envs_dirs:
  - /home/ch215616/myconda​/envs/
