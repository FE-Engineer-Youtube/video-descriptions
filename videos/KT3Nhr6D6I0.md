# ComfyUI Setup on WSL with ROCm

## Prerequisites

If you don’t have WSL installed:
[https://youtu.be/Z05cgYgqG2A](https://youtu.be/Z05cgYgqG2A)

If you don’t have ROCm installed on WSL:
[https://youtu.be/lIAQeoqSCfo](https://youtu.be/lIAQeoqSCfo)

---

## Install Dependencies

### Install Git

```bash
sudo apt install -y git
```

### Install Python 3 and pip

```bash
sudo apt install -y python3 python3-pip
```

### Install venv module

```bash
sudo apt install -y python3-venv
```

---

## Create ComfyUI Directory and Virtual Environment

```bash
mkdir -p ~/comfyui && cd ~/comfyui
python3 -m venv comfyuivenv --system-site-packages
source comfyuivenv/bin/activate
```

---

## Download PyTorch and Triton (ROCm 6.4.2)

```bash
wget https://repo.radeon.com/rocm/manylinux/rocm-rel-6.4.2/torch-2.6.0%2Brocm6.4.2.git76481f7c-cp312-cp312-linux_x86_64.whl
wget https://repo.radeon.com/rocm/manylinux/rocm-rel-6.4.2/torchvision-0.21.0%2Brocm6.4.2.git4040d51f-cp312-cp312-linux_x86_64.whl
wget https://repo.radeon.com/rocm/manylinux/rocm-rel-6.4.2/pytorch_triton_rocm-3.2.0%2Brocm6.4.2.git7e948ebf-cp312-cp312-linux_x86_64.whl
wget https://repo.radeon.com/rocm/manylinux/rocm-rel-6.4.2/torchaudio-2.6.0%2Brocm6.4.2.gitd8831425-cp312-cp312-linux_x86_64.whl
```

### Uninstall Previous Versions

```bash
pip3 uninstall torch torchvision pytorch-triton-rocm
```

### Install Downloaded Wheels

```bash
pip3 install \
  torch-2.6.0+rocm6.4.2.git76481f7c-cp312-cp312-linux_x86_64.whl \
  torchvision-0.21.0+rocm6.4.2.git4040d51f-cp312-cp312-linux_x86_64.whl \
  torchaudio-2.6.0+rocm6.4.2.gitd8831425-cp312-cp312-linux_x86_64.whl \
  pytorch_triton_rocm-3.2.0+rocm6.4.2.git7e948ebf-cp312-cp312-linux_x86_64.whl
```

### Remove Conflicting File

```bash
rm comfyuivenv/lib/python3.12/site-packages/torch/lib/libhsa-runtime64.so
```

---

## Install ComfyUI

### Clone Repository

```bash
git clone https://github.com/comfyanonymous/ComfyUI.git
```

### Remove Torch-Related Lines from Requirements

```bash
sudo nano requirements.txt
```

Delete any entries for:

- torch
- torchvision
- torchaudio

### Install ComfyUI Requirements

```bash
pip install -r requirements.txt
```

---

## Run ComfyUI

```bash
python3 main.py
```

---

## Useful Tips

### Accessing WSL Files from Windows

```
\\wsl.localhost
```

### Stop ComfyUI

```
Ctrl + C
```

### Exit Virtual Environment

```
deactivate
```

### Start ComfyUI from a Fresh Terminal

```bash
cd comfyui/ComfyUI
source ~/comfyui/comfyuivenv/bin/activate
python3 main.py
```
