Since 2025-02-04

# Enviroment setup
```
conda create -n ai_scientist python=3.11
conda activate ai_scientist
# Install pdflatex
sudo apt-get install texlive-full

# Install pypi requirements
pip install -r requirements.txt
```

为了安装正确的pytorch版本，将pytorch从requirements.txt中删除，然后运行以下命令

on DGX, Tesla V100-DGXS-32GB:cuda 12.4
```
pip3 install torch torchvision torchaudio
```

on Titan XP, cuda 11.8：
```
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

# GPU computation capability

V100-DGXS-32GB 的计算能力为7.0，Titan XP的计算能力为6.1，都不足以使用Triton，需要使用GPU的计算能力为8.0以上。

在templates/nanoGPT/experiment.py中禁用Triton：
```
import os
os.environ["TORCH_DISABLE_TRITON"] = "1"
```
train()函数中，将compile设置为False：   
```
compile=False
```
CausalSelfAttention()函数中，将self.flash设置为False：
```
self.flash = False
```