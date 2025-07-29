# unsloth_airgap
Jupyter notebook on how to download the unsloth package and migrate it to an air-gapped environment.

## Motivation
To deploy a python environment in an air-gapped environment.

## General Methodology
1. Use [uv](https://docs.astral.sh/uv/) to install the packages under `.venv`.
```bash
uv init my_project --python=3.10
cd my_project
uv add ruff # for example
# the packages will be installed under .venv folder by default
```
2. Copy the `.venv` to the air-gapped environment.
```bash
# machine with internet
tar -czvf env.tar.gz ./.venv

# air-gapped server
tar -xzvf env.tar.gz
```
3. Run the venv with one of the following
```bash
uv run your_script.py # or
source .venv/bin/activate && python your_script.py # or
.venv/bin/python your_script.py # or
```

## Handling GPU dependencies
If the packages works with GPU, like `unsloth`, it saves you the trouble if you can install the packages elsewhere in a similar environment (in particular, CUDA toolkit version).

### The rich way
1. Build a machine replicating the same setups as in the air-gapped server: the same GPUs, same OS, same GPU drivers, same CUDA toolkit version, but with internet access.
2. Install on this replicated machine, then follow the general methodology to migrate the environment. This ensures the system packages and binaries are consistent.

### The not-so-rich way
0. Note that Google Colab (or VM from other cloud providers) provides some cheap or free instance with GPU.
1. Ensure that the CUDA toolkit version on Google Colab is not newer than your air-gapped server (CUDA toolkit is backward compatible).
2. Follow the general methodology, and hope it works :sweat:.
  > For we walk by faith, not by sight. - 2 Corinthians 5:7

## Short Cut
If it happens that the following is what you need:
```
Platform: Linux
CUDA Toolkit: 12.6
Python 3.10
Torch: 2.7.1+cu126
Unsloth 2025.7.11
Transformers 4.54.0
```
The venv and its packages have been uploaded to gdrive: https://drive.google.com/file/d/11AUSUHe1hV5zF5V7VsxI8geDGomEeJmH/view?usp=drive_link
