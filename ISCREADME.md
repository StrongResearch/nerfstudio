# Nerf Studio ISC Instructions

Setup virtualenv, install requirements and download demo data

```bash
git clone https://github.com/StrongResearch/nerfstudio.git
cd nerfstudio
git checkout chris-ddp

python3 -m virtualenv .venv
source .venv/bin/activate

pip install torch==2.0.1+cu118 torchvision==0.15.2+cu118 --extra-index-url https://download.pytorch.org/whl/cu118
pip install ninja git+https://github.com/NVlabs/tiny-cuda-nn/#subdirectory=bindings/torch
pip install --upgrade pip setuptools
pip install -e .

ns-download-data nerfstudio --capture-name=poster
# ns-train nerfacto --data data/nerfstudio/poster
```

Configure for ISC by copying the following into `~/nerfstudio/config.isc`. Alternatively generate with `isc init`.

```toml
experiment_name = "nerfacto-poster"
gpu_type = "24GB VRAM GPU"
nnodes = 10
venv_path = "~/nerfstudio/.venv/bin/activate"
output_path = "~/output_nerfacto"
command = "nerfstudio/scripts/train_ddp.py nerfacto --logging.local-writer.max-log-size=0 --data data/nerfstudio/poster"
```

Begin training

```bash
isc train config.isc
```