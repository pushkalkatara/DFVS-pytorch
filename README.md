# Deep Flow Guided Scene Agnostic Image Based Visual Servoing 

This repository reproduces results from [DFVS](https://arxiv.org/abs/2003.03766) submission at [ICRA'20](https://www.icra2020.org/) for an on-going research on the problem of Visual navigation / Visual Seroving. The paper presents intersection of learning based methods and classicial robotics to visual servo a robot in 6DoF.


## Problem Statement and Approach

Visual servoing addresses the problem of attaining a desired pose with respect to a given environment using image
measurements from a monocular vision sensor.

### Approach:

(i) Optical flow as visual features predicted using [Flownet2](https://github.com/NVIDIA/flownet2-pytorch) with input as current and desired monocular images from vision sensor.  
(ii) Depth estimation using Monocular [DenseDepth](https://github.com/ialhashim/DenseDepth) with input as current monocular image from camera sensor / classical geometry estimates i.e using flow as a [proxy](http://stanford.edu/class/ee367/Winter2017/pan_ee367_win17_report.pdf) for depth.   
(iii) Flow features from (i) and Depth estimates from (ii) systematically integrated to formulate interaction matrix using [34.12](https://hal.inria.fr/hal-01355384/document)  
(iv) Generation of 6DoF control using Control Law from [34.5](https://hal.inria.fr/hal-01355384/document) solved using Levenberg-marquardt based gradient descent due to large feature vector space (optical flow).  


# Network Architecture
![Network Architecture](/media/network.png "DFVS Network Architecture")

# Simulation Environment

Performed tests in [Habitat Simulation](https://github.com/facebookresearch/habitat-sim/tree/master/habitat_sim) environment in 10 different photorealisitc environments divided into easy/medium/hard based on the goal orientation:

![Habitat Environments](/media/dataset.png "Habitat Environments")

# How to run:
1. Flownet2 Installation:
```
# get flownet2-pytorch source
git clone https://github.com/NVIDIA/flownet2-pytorch.git
cd flownet2-pytorch

# install custom layers
bash install.sh
```

2. DenseDepth Pytorch Installation
```
git clone https://github.com/ialhashim/DenseDepth
cd DenseDepth

# Download pretrained weights on NYUdepth dataset
wget https://s3-eu-west-1.amazonaws.com/densedepth/nyu.h5
```

3. Run DFVS
```
# With DenseDepth
python run_densedepth.py

# With Flow as proxy for depth
python run_flowdepth.py
```

# Results
Environment: Skcastle | Left: Goal Image | Right: DFVS  
![Environment: Skcastle | Left: Goal Image | Right: DFVS ](/media/skcastle.gif)  
Environment: Quantico | Left: Goal Image | Right: DFVS  
![Environment: Quantico | Left: Goal Image | Right: DFVS ](/media/quantico.gif)  
Environment: Ballou | Left: Goal Image | Right: DFVS  
![Environment: Ballou | Left: Goal Image | Right: DFVS ](/media/ballou.gif)  
Environment: Mesic | Left: Goal Image | Right: DFVS  
![Environment: Mesic | Left: Goal Image | Right: DFVS ](/media/mesic.gif)  
