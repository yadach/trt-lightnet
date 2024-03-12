# TensorRT-LightNet: High-Efficiency and Real-Time CNN Implementation on Edge AI

trt-lightNet is a CNN implementation optimized for edge AI devices that combines the advantages of LightNet <sup>[[1]](#references)</sup> and TensorRT <sup>[[2]](#references)</sup>. LightNet is a lightweight and high-performance neural network framework designed for edge devices, while TensorRT is a high-performance deep learning inference engine developed by NVIDIA for optimizing and running deep learning models on GPUs. LightNet-TRT uses the Network Definition API provided by TensorRT to integrate LightNet into TensorRT, allowing it to run efficiently and in real-time on edge devices.
This is a reproduction of lightNet-TRT <sup>[[6]](#references)</sup>, which generates a TensorRT engine from the ONNX format.

## Key Improvements

### 2:4 Structured Sparsity

LightNet-TRT utilizes 2:4 structured sparsity <sup>[[3]](#references)</sup>  to further optimize the network. 2:4 structured sparsity means that two values must be zero in each contiguous block of four values, resulting in a 50% reduction in the number of weights. This technique allows the network to use fewer weights and computations while maintaining accuracy.

![Sparsity](https://developer-blogs.nvidia.com/ja-jp/wp-content/uploads/sites/6/2022/06/2-4-structured-sparse-matrix.png "sparsity")

### NVDLA Execution

LightNet-TRT also supports the execution of the neural network on the NVIDIA Deep Learning Accelerator (NVDLA) <sup>[[4]](#references)</sup> , a free and open architecture that provides high performance and low power consumption for deep learning inference on edge devices. By using NVDLA, LightNet-TRT can further improve the efficiency and performance of the network on edge devices.

![NVDLA](https://i0.wp.com/techgrabyte.com/wp-content/uploads/2019/09/Nvidia-Open-Source-Its-Deep-Learning-Inference-Compiler-NVDLA-2.png?w=768&ssl=1 "NVDLA")


### Multi-Precision Quantization

In addition to post training quantization <sup>[[5]](#references)</sup>, LightNet-TRT also supports multi-precision quantization, which allows the network to use different precision for weights and activations. By using mixed precision, LightNet-TRT can further reduce the memory usage and computational requirements of the network while still maintaining accuracy. By writing it in CFG, you can set the precision for each layer of your CNN.

![Quantization](https://developer-blogs.nvidia.com/wp-content/uploads/2021/07/qat-training-precision.png "Quantization")



### Multitask Execution (Detection/Segmentation)

LightNet-TRT also supports multitask execution, allowing the network to perform both object detection and segmentation tasks simultaneously. This enables the network to perform multiple tasks efficiently on edge devices, saving computational resources and power.

[![](https://img.youtube.com/vi/TmlW-b_t3sQ/0.jpg)](https://www.youtube.com/watch?v=TmlW-b_t3sQ)

## Installation

### Requirements

-   CUDA 11.0 or higher
-   TensorRT 8.0 or higher
-   OpenCV 3.0 or higher

### Steps

1.  Clone the repository.
    
```shell
$ git clone XXX
$ cd trt-lightnet
```
	
2.  Install libraries.
						    
```shell
$ sudo apt update
$ sudo apt install libgflags-dev
$ sudo apt install libboost-all-dev
```
										    
3.  Compile the TensorRT implementation.
											    
```shell
$ mkdir build
$ cmake ../
$ make -j
```

## Model	
 T.B.D
 
## Usage

### Converting a LightNet model to a TensorRT engine
						
Build FP32 engine
```shell
$ ./trt-lightnet --flagfile ../configs/CONFIGS.txt --precision fp32
```

Build FP16(HALF) engine
```shell
$ ./trt-lightnet --flagfile ../configs/CONFIGS.txt --precision fp16
```

Build INT8 engine 
(You need to prepare a list for calibration in "models/calibration_images.txt".)
```shell
$ ./trt-lightnet --flagfile ../configs/CONFIGS.txt --precision int8 --first true
```
First layer is much more sensitive for quantization.
Threfore, the first layer is not quanitzed using "--first true"

Build DLA engine (Supported by only Xavier and Orin)
```shell
$ ./trt-lightnet --flagfile ../configs/CONFIGS.txt --precision int8 --first true --dla [0/1]
```

### Inference with the TensorRT engine

Inference from images
```shell
$ ./trt-lightnet --flagfile ../configs/CONFIGS.txt --precision [fp32/fp16/int8] --first true {--dla [0/1]} --d DIRECTORY
```

Inference from images
```shell
$ ./trt-lightnet --flagfile ../configs/CONFIGS.txt --precision [fp32/fp16/int8] --first true {--dla [0/1]} --v VIDEO
```

## Implementation

LightNet-TRT is built on the LightNet framework and integrates with TensorRT using the Network Definition API. The implementation is based on the following repositories:

-   LightNet: [https://github.com/daniel89710/lightNet](https://github.com/daniel89710/lightNet)
-   TensorRT: [https://github.com/NVIDIA/TensorRT](https://github.com/NVIDIA/TensorRT)
-   NVIDIA DeepStream SDK: [https://github.com/NVIDIA-AI-IOT/deepstream\_reference\_apps/tree/restructure](https://github.com/NVIDIA-AI-IOT/deepstream_reference_apps/tree/restructure)
-   YOLO-TensorRT: [https://github.com/enazoe/yolo-tensorrt](https://github.com/enazoe/yolo-tensorrt)
-   trt-yoloXP: [https://github.com/tier4/trt-yoloXP]

## Conclusion

trt-lightnet is a powerful and efficient implementation of CNNs using Edge AI. With its advanced features and integration with TensorRT, it is an excellent choice for real-time object detection and semantic segmentation applications on edge devices.

# References
[1]. [LightNet](https://github.com/daniel89710/lightNet)  
[2]. [TensorRT](https://developer.nvidia.com/tensorrt)  
[3]. [Accelerating Inference with Sparsity Using the NVIDIA Ampere Architecture and NVIDIA TensorRT](https://developer.nvidia.com/blog/accelerating-inference-with-sparsity-using-ampere-and-tensorrt/)  
[4]. [NVDLA](http://nvdla.org/)  
[5]. [Achieving FP32 Accuracy for INT8 Inference Using Quantization Aware Training with NVIDIA TensorRT](https://developer.nvidia.com/blog/achieving-fp32-accuracy-for-int8-inference-using-quantization-aware-training-with-tensorrt/)  
[6]. [lightNet-TR](https://github.com/daniel89710/lightNet-TRT)

