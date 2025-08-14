**Table Of Contents**

[1 CREATING AN INSTANCE WITH BECHTLE-VSR AMI](#1-creating-an-instance-with-bechtle-vsr-ami)<br/>
[2 TRY OUR SR SOLUTIONS](#2-try-our-sr-solution)<br/>
[3 REFERENCES](#3-references)<br/>

---

# 1 Creating an instance with Bechtle-VSR AMI

## PROCEDURE

### Step 1. Choose the AMI we shared

- Click “Launch instance” in EC2 console
- Type "Bechtle-VSR" in the search box and select it.

### Step 2. Choose g4dn.2xlarge instance

choose g4dn.2xlarge
<br/>
<img src="images/aws_choose_g4dn.2xlarge.png" width="70%">
<br/>

### Step 3. Review and launch the instance

- Create a private key if you have no existing key and download it.
  <br/>
  <img src="images/creating_private_key.png" width="30%">
  <br/>
- Click "Launch Instances"

### Step 4. Connect to your instance

```bash
chmod 600 <private_key_path>
ssh -i <private_key_path> ubuntu@<ip_address>
```

Then you can see the following messages:

```

 ____  _____ ____ _   _ _____ _     _____   __     ______  ____
| __ )| ____/ ___| | | |_   _| |   | ____|  \ \   / / ___||  _ \
|  _ \|  _|| |   | |_| | | | | |   |  _| ____\ \ / /\___ \| |_) |
| |_) | |__| |___|  _  | | | | |___| |__|_____\ V /  ___) |  _ <
|____/|_____\____|_| |_| |_| |_____|_____|     \_/  |____/|_| \_\

                                                  https://blue-dot.io
                                                  contact@blue-dot.io

#### HOWTO ####

bluedot.sh 720p_musicvideo.mp4 result_x2.mp4 2
bluedot.sh 720p_musicvideo.mp4 result_x3.mp4 3

sample clips
- 720p_sports.mp4
- 720p_musicvideo.mp4
```

# 2 Try our SR solution

### Using easy script(bluedot.sh).

```bash
### 2x SR #######
bluedot.sh 720p_musicvideo.mp4 result_x2.mp4 2

### 3x SR #######
bluedot.sh 720p_musicvideo.mp4 result_x3.mp4 3
```

#### note) Use Bechtle v3_nr(previous model)

Bechtel v5 (latest model) is the default, but you can change by setting the model parameter.

```bash
### 2x SR #######
bluedot.sh 720p_musicvideo.mp4 result_x2.mp4 2 bechtle_v3_nr

### 3x SR #######
bluedot.sh 720p_musicvideo.mp4 result_x2.mp4 2 bechtle_v3_nr
```

#### Select GPU to use

If using multi-GPU instances such as g4dn.12xlarge or g4dn.metal, you can specify which GPU to use.

- CUDA_VISIBLE_DEVICES starts from 0, with a maximum value of the total number of GPUs minus 1.

```bash
### Use 1st GPU
CUDA_VISIBLE_DEVICES=0 bluedot.sh 720p_musicvideo.mp4 result_x2.mp4 2

### Use 2nd GPU
CUDA_VISIBLE_DEVICES=1 bluedot.sh 720p_musicvideo.mp4 result_x3.mp4 3
```

### Using ffmpeg directly.

```bash
### 2x SR #######
ffmpeg -hide_banner -y -sws_flags spline+accurate_rnd+full_chroma_int -i 720p_musicvideo.mp4 -vf bdsr_aws=scale=2,scale=out_color_matrix=bt709 -pix_fmt yuv420p -colorspace bt709 -c:v libx264 resutl_x2.mp4

### 3x SR #######
ffmpeg -hide_banner -y -sws_flags spline+accurate_rnd+full_chroma_int -i 720p_musicvideo -vf bdsr_aws=scale=3,scale=out_color_matrix=bt709 -pix_fmt yuv420p -colorspace bt709 -c:v libx264 resutl_x3.mp4
```

#### Select GPU to use

Similar to the bluedot.sh script, you can also set CUDA_VISIBLE_DEVICES=X before executing ffmpeg commands to select a specific GPU.

# 3 References

## Using the terminal in VS Code

Because the terminal in VS Code starts as a non-login shell, run the following command:

```
bash -l
```

# 4 Live Use Cases for Bluewhale-VSR AMI with AWS Elemental

The Bluewhale-VSR AMI is a GPU-based AMI that performs real-time video quality enhancement for low-resolution videos, including upscaling and noise reduction. This document describes two representative live integration use cases within the AWS Elemental environment (MediaConnect, MediaLive, MediaConvert).

### [Use case A : MediaConnect <-> Bluewhale-VSR <-> MediaLive](./use-cases/live.md)

### [Use case B : S3 -> Bluewhale-VSR AMI -> MediaConvert](./use-cases/vod.md)
