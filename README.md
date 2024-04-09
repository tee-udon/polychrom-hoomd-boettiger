# polychrom-hoomd-boettiger

This repository provides a guide to use `polychrom-hoomd` package to perform polymer simulation on Sherlock computational clusters. 

## Installation

1. **Login to Sherlock OnDemand session** 
   1. Login to [your Sherlock OnDemand session](https://ondemand.sherlock.stanford.edu/). 
      - Two factor authentication is required to login to your Sherlock account. 
  
   2. On the top panel, click _Clusters_ and choose _Sherlock Shell Access_. This will launch a shell session for your Sherlock account.

2. **Installing `mamba`. Skip this part if you have install `mamba` on your Sherlock account.** 

     `mamba` is a Python package management similar to `pip` and a more closer relative `conda`. The problem with `conda` is that it can be clunky when it checks package dependencies before installing them. `mamba` is a lightweight alternative that speeds the installation and dependencies verification significantly.  

   1. To install `mamba`, run the following in your recently open Shell. More information about `mamba` installation can be found [here](https://mamba.readthedocs.io/en/latest/installation/mamba-installation.html) and [here](https://github.com/conda-forge/miniforge). 

        ```bash 
        curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
        
        bash Miniforge3-$(uname)-$(uname -m).sh
        ```
   2. Activate conda shell function by running the following command. Replace `<your SUNet ID>` with your SUNet ID. 
   
        ```bash 
        eval "$(/home/users/<your SUNet ID>/miniforge3/bin/conda shell.bash hook)"
        ```
        

   3. Test if the conda shell function has been activated by running 
        ```bash
        which mamba
        ```
        It should return a path to your `miniforge3` installation. For example, 

        ```bash 
        ~/miniforge3/bin/mamba
        ```

3. **Install a virtual environment and required packages.**
    
    Virtual environments are an important part of a good coding practice. Without them, Python packages with different requirements and dependencies can contradict one another and produce many downstream problems. These virtual environments provide compartmentalization for packages such that packages in different environments do not interact with one another. **Therefore, it is a good practice to create a new virtual environment for a new coding project.**

    1. Create a virtual environment, named `polychrom` with `python` version 3.10, by running the following command. 
        
        ```bash
        mamba create -n polychrom python=3.10
        ```

    2. Activate this newly created virtual environment by running 
        ```bash
        mamba activate polychrom
        ```
        The text in front of your Shell prompt should change from `(base)` to `(polychrom)`. 

    3. Install necessary packages by enter the following line 
        ```bash
        mamba install pip numpy=1.23 scipy=0.16 matplotlib pandas fresnel gsd joblib pyside ipython notebook 

        pip install git+https://github.com/open2c/polykit.git git+https://github.com/open2c/cooltools.git git+https://github.com/mtortora/lattice-translocators.git
        ```

    4. Then, we need to install `hoomd` with `GPU` capabilities. Run the following command.
        ```bash
        export CONDA_OVERRIDE_CUDA="12.0"
        
        conda install "hoomd=4.6.0=*gpu*" "cuda-version=12.0"
        ```
    
    5. Lastly, we need to install `polychrom-hoomd` package by enter the following command.
        ```bash
        git clone https://github.com/open2c/polychrom-hoomd.git

        cd polychom-hoomd

        python -m pip install -e . 
        ```

4. **Install Python kernel for OnDemand JupyterLab session.** 

    If you have used `Jupyter Notebook` or `Jupyter Lab` before, the Python interactivity in `Jupyter` originates from Python kernel. In this case, we want to install Python kernel from this `polychrom` environment we just created so that we can access all the packages we have spent time install in the previous section. 

    1. Create a Python kernel name polychrom.
        ```bash
        python -m ipykernel install --user --name polychrom --display-name "polychrom"
        ```

5. **Run Jupyter Interactive Session on Sherlock OnDemand.**
   
   1. In your OnDemand session, go to *Interactive Apps* and choose *JupyterLab*. 
   2. In *Additional modules (optional)* section, enter
        ```
        cuda/12.4
        ```
   3. Select `gpu` *partition*.
   4. Choose >= 1 *#GPUs*.
   5. Check *I would like to receive an email when the session starts* for the notification when the interative session starts. 
   6. Click *Launch*. 
   7. After the session starts, the `polychrom` kernel that we just created will not work right away due to its interaction with OnDemand session. We just have to locate the *Modules* tab to the left (it should look like a blue cube). Unload `python` and `py-jupyterlab`, then restart `polychrom` kernel. If nothing goes wrong, `polychrom | Idle` should show up on the status bar in the lower left part of the Jupyter Lab.

## Tutorial 
1. Juputer Notebook tutorials can be found in `polychrom-hoomd` folder we just cloned. For example, in
   ```bash
   ~/polychrom-hoomd/examples/
   ```