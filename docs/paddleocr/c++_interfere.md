#### BackGround


PaddleOcr is a open source and free of charge to use to detect and event regconize text with alot of advantagies like high precise, fast detect, support more than 80 languages, support both python and C++ programming language.


#### Purpose


This doccument will describe how we can Build & Run a simple C++ demo from PaddleOcr repository.

#### Environment


Ubuntu 18.04


#### Install minimal prerequisites (Ubuntu 18.04 as reference)


```bash

sudo apt update && sudo apt install -y cmake g++ wget unzip

```


#### Pull the PaddleOcr repo


```bash

git clone https://github.com/PaddlePaddle/PaddleOCR.git

```

The source will be place at folder **PaddleOCR**


#### Compile the OpenCV 


Firstly, download the OpenCV source code. In this tutorial, we use version 3.4.7.

```bash

cd ..PaddleOCR/deploy/cpp_infer
wget https://paddleocr.bj.bcebos.com/libs/opencv/opencv-3.4.7.tar.gz
tar -xf opencv-3.4.7.tar.gz

```

By default, Paddle provide a script to install **opencv-3.4.7** at:

```bash

../PaddleOCR/PaddleOCR/deploy/cpp_infer/tools/build_opencv.sh

```

Open & edit the **root_path** to match with your path:

```bash

root_path=/home/andy/work/PaddleOCR/deploy/cpp_infer/opencv-3.4.7
install_path=${root_path}/opencv3
build_dir=${root_path}/build

rm -rf ${build_dir}
mkdir ${build_dir}
cd ${build_dir}

cmake .. \
    -DCMAKE_INSTALL_PREFIX=${install_path} \
    -DCMAKE_BUILD_TYPE=Release \
    -DBUILD_SHARED_LIBS=OFF \
    -DWITH_IPP=OFF \
    -DBUILD_IPP_IW=OFF \
    -DWITH_LAPACK=OFF \
    -DWITH_EIGEN=OFF \
    -DCMAKE_INSTALL_LIBDIR=lib64 \
    -DWITH_ZLIB=ON \
    -DBUILD_ZLIB=ON \
    -DWITH_JPEG=ON \
    -DBUILD_JPEG=ON \
    -DWITH_PNG=ON \
    -DBUILD_PNG=ON \
    -DWITH_TIFF=ON \
    -DBUILD_TIFF=ON

make -j12
make install

```

Run command to build and install:

```bash

cd deploy/cpp_infer/
chmod +x ./tools/build_opencv.sh
./tools/build_opencv.sh

```

It'll take a while to compile. When build complete, opencv will be placed at 

```bash

../deploy/cpp_infer/opencv-3.4.7/build/opencv3

```


#### Compile or Download the Paddle Inference Library


There are 2 ways to obtain the Paddle inference library, described in paddle homepage.

In this tutorial, we use the pre-build library for X64 OS.
Access this link to download libs:
https://paddleinference.paddlepaddle.org.cn/user_guides/download_lib.html#linux

Or
```bash

wget https://paddle-inference-lib.bj.bcebos.com/2.3.0/cxx_c/Linux/CPU/gcc5.4_avx_mkl/paddle_inference.tgz

```

Extract
```bash

cd ../deploy/cpp_infer/tools
tar -xf paddle_inference.tgz

```

Update libs path to the build scrip at **../deploy/cpp_infer/tools/build.sh**.

```bash

OPENCV_DIR=/home/andy/work/PaddleOCR/deploy/cpp_infer/opencv-3.4.7/opencv3
LIB_DIR=/home/andy/work/PaddleOCR/deploy/cpp_infer/tools/paddle_inference
CUDA_LIB_DIR=your_cuda_lib_dir
CUDNN_LIB_DIR=your_cudnn_lib_dir

BUILD_DIR=build
rm -rf ${BUILD_DIR}
mkdir ${BUILD_DIR}
cd ${BUILD_DIR}
cmake .. \
    -DPADDLE_LIB=${LIB_DIR} \
    -DWITH_MKL=ON \
    -DWITH_GPU=OFF \
    -DWITH_STATIC_LIB=OFF \
    -DWITH_TENSORRT=OFF \
    -DOPENCV_DIR=${OPENCV_DIR} \
    -DCUDNN_LIB=${CUDNN_LIB_DIR} \
    -DCUDA_LIB=${CUDA_LIB_DIR} \
    -DTENSORRT_DIR=${TENSORRT_DIR} \

make -j12


```

Finally build the C++ demo:

```

cd ../deploy/cpp_infer/
sh tools/build.sh

```

It takes just some second to complete. The target file is locate at **../deploy/cpp_infer/build/ppocr**

Note: If error *-lz libs not found* occus, try install it:

```bash

sudo apt-get install -y libz-dev

```

