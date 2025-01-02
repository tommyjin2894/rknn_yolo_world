# YOLO-WORLD with RKNN(aio3588q board)
### RKNN System Install
```
- board : aio3588q
- Flash the firmware onto the device first.
```
- in ubuntu (bash)
    ```shell
    sudo apt update && sudo apt upgrade -y
    sudo apt install -y build-essential curl wget git vim nano unzip zip \
    htop net-tools openssh-server software-properties-common

    sudo apt install -y snapd
    sudo apt install chromium-browser

    sudo apt install make
    sudo apt autoremove -y && sudo apt autoclean
    ```

- Install Conda
    ```shell
    bash Anaconda3-2024.10-1-Linux-aarch64.sh 
    ```

- Install VSCode
    ```shell
    sudo dpkg -i code_1.96.2-1734606918_arm64.deb 
    ```

### Set up the RKNN Environment
https://docs.radxa.com/en/rock5/rock5c/app-development/rknn_install

```bash
# Create Projects folder
mkdir Projects
cd Projects

# Download RKNN-Toolkit2 repository
git clone https://github.com/airockchip/rknn-toolkit2.git

# Download RKNN Model Zoo repository
git clone https://github.com/airockchip/rknn_model_zoo.git
```

make env
```shell
conda create -n rknn python=3.8
conda activate rknn
```

install toolkit2
```shell
# Choose the appropriate requirements file according to your python version
pip install -r Projects/rknn-toolkit2/rknn-toolkit2/packages/arm64/arm64_requirements_cp38.txt
pip install Projects/rknn-toolkit2/rknn-toolkit2/packages/arm64/rknn_toolkit2-2.3.0-cp38-cp38-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
```

Install Toolkit2-Lite
```shell
pip install Projects/rknn-toolkit2/rknn-toolkit-lite2/packages/rknn_tool
kit_lite2-2.3.0-cp38-cp38-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
```

Install ETC
https://github.com/radxa-pkg/rknn2/releases
```

sudo apt-get update
sudo apt-get install -y python3-ruamel.yaml python3-psutil python3-numpy

sudo dpkg -i --force-overwrite python3-rknnlite2_2.3.0-1_arm64.deb
sudo dpkg --install --force-overwrite rknpu2-rk3588_2.3.0-1_arm64.deb
```

### RKNN Model Zoo Testing YOLO World (with CLIP Model)
```shell
pip install transformers
```
```shell
cd Projects/rknn_model_zoo/examples/yolo_world/model/
```

```shell
wget -O ./clip_text.onnx https://ftrg.zbox.filez.com/v2/delivery/data/95f00b0fc900458ba134f8b180b3f7a1/examples/clip/clip_text.onnx
wget -O ./yolo_world_v2s.onnx https://ftrg.zbox.filez.com/v2/delivery/data/95f00b0fc900458ba134f8b180b3f7a1/examples/yolo_world/yolo_world_v2s.onnx
```

- The CLIP text model only supports FP and not INT8
```shell
cd ../../python/clip_text/
python convert.py ../../model/clip_text.onnx rk3588 fp
```
- The Yolo World model (default : INT8)
```shell
cd ../../python/yolo_world/
python convert.py ../../model/yolo_world_v2s.onnx rk3588
```

- Run Models with Python
```shell
cd ..
python yolo_world.py --target rk3588
```

- It runs properly
```text
   class        score      xmin, ymin, xmax, ymax
--------------------------------------------------
   person       0.948     [ 477,  232,  559,  521]
   person       0.932     [ 110,  237,  226,  535]
   person       0.917     [ 211,  242,  283,  508]
   person       0.595     [  80,  326,  125,  514]
    bus         0.917     [  98,  135,  553,  436]
```

- result img custom text and custom img
![img](result.jpg)