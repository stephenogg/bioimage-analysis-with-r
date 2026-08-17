---
title: "Setup"
---

## Overview

Before starting this lesson, please complete the software and data setup described below.

This lesson uses:

- JupyterLab
- R
- An R kernel for Jupyter
- The `EBImage` and `tidyverse` libraries to extend base R.

We will use a Python virtual environment created by Conda to install and manage all 
software and  dependencies required for the lesson. We will not use an R integrated 
development environment, like RStudio. Rather we will enter commands into a JuPyteR
notebook that will execute the R commands using an R kernel instead of a Python kernel.


## Data

---------

The example images and a description of the Python environment (`environment.yml`) 
used in this lesson are available on Github. To download the data, 
please visit the dataset page for this workshop and click the “Download all” button. 
Unzip the downloaded file, and save the contents as a folder called *data* 
somewhere you will easily find it again, e.g. your Desktop or a folder you have created 
for use during this workshop. The name *data* is optional but recommended, 
as this is the name we will use to refer to the folder throughout the lesson.


### Download the environment file:

The complete Conda environment specification is available at:

- [environment.yaml](https://github.com/stephenogg/bioimage-analysis-with-r/blob/main/environment.yaml)

- [data](https://raw.githubusercontent.com/stephenogg/bioimage-analysis-with-r/main/episodes/data.zip)

and extract it into a folder named: `bioimage-r`

We recommend the following directory structure:

```text
bioimage-r/
|
├── data/
├── environment.yml
└── notebooks/
```

## Software

### Step 1: Install Miniforge

Install Miniforge for your operating system:

https://conda-forge.org/download/

Miniforge provides the Conda package manager that we will use to create the virtual environment.

The next steps assumes that `conda` is available to manage your Python environment. After installation, verify Conda is available.

::: tab

### Windows

Open:

```text
Miniforge Prompt
```


Run:

```bash
conda --version
```

### MacOS

Open Terminal and run:

```bash
conda --version
```

### Linux

Open a terminal and run:

```bash
conda --version
```
:::

You should see a Conda version number - e.g. `conda 26.1.1`

## Step 2: Create the Virtual Lesson Environment

- Download the environment description file:

```text
environment.yml
```

and place it in your working directory. (This file should already be downloaded, 
if you followed the instructions above.)

- Create the lesson environment.

::: tab

### Windows

Open Miniforge Prompt and run:

```bash
conda env create -f environment.yml
```

### MacOS

Open Terminal and run:

```bash
conda env create -f environment.yml
```

### Linux

Open a terminal and run:

```bash
conda env create -f environment.yml
```

:::

This will create a python virtual environment named `bioimage-r`

and install:

- Python
- JupyterLab
- R
- IRkernel
- tidyverse
- EBImage

The installation may take several minutes. Click *OK* if your computer asks you 
to approve any installations.

## Step 3: Activate the Environment

::: tab

### Windows

```bash
conda activate bioimage-r
```

### MacOS

```bash
conda activate bioimage-r
```

### Linux

```bash
conda activate bioimage-r
```
:::

Before activating the virtual environment, your command prompt should begin with `(base)`.
After activation, your command prompt should now begin with the name of the active virtual 
environment in parentheses, e.g: `bioimage-r`.

## Step 4: Launch JupyterLab

::: tab

### Windows

From Miniforge Prompt:

```bash
jupyter lab
```

### MacOS

From Terminal:

```bash
jupyter lab
```

### Linux

From Terminal:

```bash
jupyter lab
```
:::

A bunch of text will start to appear in your terminal as the jupyther lab starts up. 
JupyterLab *should* open automatically in your web browser. 
If it does not, you may see a link to click towards the end of the text that appears 
in your terminal.

## Step 5: Create an R Notebook

In JupyterLab:

The link should open to a ![JuPyteR Launcher](fig/00-setup/JupyterLauncher.png){alt="launcher page from a newly deployed JuPyteR Lab"}

- Click on the R button to open a new, untitled notebook running an R kernel.

If there is no launcher, then:

1. Select **File → New → Notebook** (or **File → New → Launcher**)
2. Choose the **R** kernel
3. A new notebook should start in a new tab. 

If an R kernel is not available, consult the troubleshooting section below.

## Testing Your Installation

### Running Cells

To run code in JupyterLab:

1. Click inside a code cell.
2. Press `Shift + Enter`.

The cell will execute and display the result.

Create a notebook cell, copy the code below into the cell and run it (`shift + enter`):

```r
library(EBImage)

img <- Image(
  matrix(
    runif(10000),
    nrow = 100
  )
)

display(img)
```

If a grayscale image appears and no error message is generated, your
installation is working correctly.

### Test Required Packages

Run:

```r
library(tidyverse)
library(EBImage)

sessionInfo()
```

Verify that:

- R loads successfully
- tidyverse loads successfully
- EBImage loads successfully

### Test Data Access

Run:

```r
list.files("data")
```

You should see the contents of the lesson data directory.



::::::::::::::::  spoiler

## Running Cells in a Notebook

![Overview of the Jupyter Notebook graphical user interface](fig/00-setup/jupyter_overview.png){alt="explanatin of cells in a JuPyteR notebook"}
To run code in a Jupyter notebook cell, click on a cell in the notebook
(or add a new one by clicking the `+` button in the toolbar),
make sure that the cell type is set to "Code" (check the dropdown in the toolbar),
and add the R code in that cell. After you have added the code,
you can run the cell by selecting "Run" -> "Run selected cell" in the top menu,
or pressing <kbd>Shift</kbd>\+<kbd>Enter</kbd>.



:::::::::::::::::::::::::

## Troubleshooting

### Conda Not Found

If: <kbd>conda --version</kbd> returns an error, restart your terminal after installing Miniforge.

Windows users should ensure they are using **Miniforge Prompt**.

### JupyterLab Will Not Start

Verify:

```bash
jupyter --version
```

If no version number is displayed, recreate the Conda environment:

```bash
conda env create -f environment.yml
```

### R Kernel Not Listed

Run:

```bash
jupyter kernelspec list
```

You should see an entry for R.

If no R kernel appears, recreate the Conda environment.

### EBImage Will Not Load

Activate the lesson environment:

```bash
conda activate bioimage-r
```

Then launch JupyterLab again.



A small number of exercises will require you to run commands in a terminal. Windows users should 
use PowerShell for this. PowerShell is probably installed by default but if not you should
[download and install](https://apps.microsoft.com/detail/9MZ1SNWT0N5D?hl=en-eg&gl=EG) it.

