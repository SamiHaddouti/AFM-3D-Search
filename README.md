# 3D Search

## Table of contents
* [Introduction](#introduction)
* [Getting Started](#getting-started)
* [Argument Reference](#arguments)
* [Repository Structure](#structure)
* [Helpful commands](#helpful-commands)


## Introduction
This repository contains our current implementation of a Synthesis Prediction Model as part of our TUM.ai impact project (Batch WS 2024/25).

## Getting started
We recommend using [conda](https://docs.conda.io/docs/user-guide/install/) to install the necessary environment for this repository. Follow the instructions on the conda installation page to set up conda on your system.

All necessary dependencies are listed in the `requirements.txt` file. You can create a new conda environment using the following command:

```bash
conda create --name <env_name> --file requirements.txt
```

Next, clone the repository using the following command:

```bash
git clone https://github.com/yourusername/MIT-Impact-Project.git
```

Finally, you can run the training/eval script with the following command:

```bash
python main.py --some_flag ARG_VALUE
```

Please refer to the arguments section below for possible command line arguments.

## Arguments
| Flag        | Explanation                                | Default |
|-------------|--------------------------------------------|---------|
| --sim_emb   | help="Use similarity ranking with meanpooled embeddings" | True |
| --lr        | Learning rate                              | 1e-4    |
| --epochs    | Number of epochs                           | 100     |
| --batch_size| Batch size                                 | 16      |
| --split_ratio| Train/eval split ratio                    | 0.8     |
| --k         | Top-k value for calculating Top-k accuracy | 10      |
| --margin    | Margin for the rank loss                   | 100.0   |
| --output_dir| Path where to save, empty for no saving    | ""      |

## Structure
The repository is structured as follows:

```
MIT-Impact-Project/
├── archive/                # Archive for previous versions and deprecated stuff
├── data/                   # Directory containing datasets
├── models/                 # Directory containing model definitions, datasets, and the metrics/losses
├── notebooks/              # Jupyter notebooks for experiments and analysis
├── run/                    # Create by tensorboard for storing/visualizing log data
├── .gitignore              # Specifies files to be ignored
├── engine.py               # Training and Evaluation Loops
├── main.py                 # Main script for argument parsing, initialization, logging
├── README.md               # Project documentation
└──requirements.txt         # File listing all dependencies (for conda)
```

## Helpful commands
Export updated conda environment (CAUTION: will overwrite current requirements.txt):
```bash
 conda list -e > requirements.txt
```
