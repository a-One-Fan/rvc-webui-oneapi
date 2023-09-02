
# Retrieval-based-Voice-Conversion-WebUI (oneAPI version)

[Demo Video of non-oneAPI version](https://www.bilibili.com/video/BV1pm4y1z7Gm/)

oneAPI fork of w-okada RVC, for inference: https://github.com/a-One-Fan/okada-rvc-oneapi

Currently, this (wil be) tested on WSL2. Expect a more automated installation in the future?

# Installation:

- Install the oneAPI base toolkit

https://www.intel.com/content/www/us/en/developer/tools/oneapi/base-toolkit-download.html?operatingsystem=linux&distributions=aptpackagemanager

- Get conda

Go to https://repo.anaconda.com/archive/

Copy the link for the newest conda (e.g. the one called "Anaconda3-2023.07-2-Linux-x86_64.sh"), `wget` it, run it and install. Don't forget to `chmod` it if it refuses to run.

- Create a conda environment, install this repo's requirements

This repo requires torchaudio. Intel do not officially provide torchaudio wheels. You may get my compiled wheels from mega.nz, via megatools in the command line.
From inside this repo's folder,

```sh
conda create -n rvc python=3.10
conda activate rvc_webui
sudo apt install libportaudio2 libasound-dev megatools
pip install astunparse numpy==1.23.5 pyyaml>=6.0 pytest psutil setuptools cffi typing_extensions future six requests hypothesis expecttest types-dataclasses dataclasses Pillow>=9.1.1 SoundFIle==0.12.1 kaldi-io==0.9.8 scipy==1.11.2
mkdir megatemp
cd ./megatemp
megadl https://mega.nz/file/ndIRTYzQ#tn8teb5dXZQ6GNJBfRxykI1zQ58f-xBu8wNi-usYYho
tar -xzvf ./wheels.tar.gz
python -m pip install torch-2.0.1a0+gite9ebda2-cp310-cp310-linux_x86_64.whl
python -m pip install --no-deps torchvision-0.15.2a0+fa99a53-cp310-cp310-linux_x86_64.whl
python -m pip install --no-deps torchaudio-2.0.2+31de77d-cp310-cp310-linux_x86_64.whl
intel_extension_for_pytorch-2.0.110+gite29c5fb-cp310-cp310-linux_x86_64.whl
cd ..
rm -rf ./megatemp
cd ./server
pip install -r requirements.txt
cd ..
```

If you do not wish to get my wheels, you can compile them yourself with Intel's convenient compile script: https://github.com/intel/intel-extension-for-pytorch/blob/xpu-master/scripts/compile_bundle.sh

This will take a while.

- Get pretrained models

https://huggingface.co/lj1995/VoiceConversionWebUI/tree/main/

List of needed models and other needed files?
```bash
./assets/hubert/hubert_base.pt

./assets/pretrained 

./assets/uvr5_weights

Additional downloads are required if you want to test the v2 version of the model.

./assets/pretrained_v2

If you want to test the v2 version model (the v2 version model has changed the input from the 256 dimensional feature of 9-layer Hubert+final_proj to the 768 dimensional feature of 12-layer Hubert, and has added 3 period discriminators), you will need to download additional features

./assets/pretrained_v2

If you want to use the latest SOTA RMVPE vocal pitch extraction algorithm, you need to download the RMVPE weights and place them in the RVC root directory

https://huggingface.co/lj1995/VoiceConversionWebUI/blob/main/rmvpe.pt

```

- Create an alias to source oneAPI environment scipts easily

```sh
echo 'alias oneapi="source /opt/intel/oneapi/mkl/latest/env/vars.sh; source /opt/intel/oneapi/compiler/latest/env/vars.sh"' >> ~/.bash_aliases
```

# Run?

```bash
conda activate rvc_webui
oneapi
python infer-web.py
```

## Credits

+ [Retrieval-based-Voice-Conversion-WebUI](https://github.com/a-One-Fan/rvc-webui-oneapi)
+ [ContentVec](https://github.com/auspicious3000/contentvec/)
+ [VITS](https://github.com/jaywalnut310/vits)
+ [HIFIGAN](https://github.com/jik876/hifi-gan)
+ [Gradio](https://github.com/gradio-app/gradio)
+ [FFmpeg](https://github.com/FFmpeg/FFmpeg)
+ [Ultimate Vocal Remover](https://github.com/Anjok07/ultimatevocalremovergui)
+ [audio-slicer](https://github.com/openvpi/audio-slicer)
+ [Vocal pitch extraction:RMVPE](https://github.com/Dream-High/RMVPE)
  + The pretrained model is trained and tested by [yxlllc](https://github.com/yxlllc/RMVPE) and [RVC-Boss](https://github.com/RVC-Boss).
  
## Thanks to all contributors for their efforts
<a href="https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI/graphs/contributors" target="_blank">
  <img src="https://contrib.rocks/image?repo=RVC-Project/Retrieval-based-Voice-Conversion-WebUI" />
</a>

