# python opencv

### step to install
1. install dependencies
```
sudo apt update && \
  sudo apt install -y build-essential cmake git unzip pkg-config make libavcodec-dev libavformat-dev libswscale-dev libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev libgtk-3-dev libpng-dev libjpeg-dev libopenexr-dev libtiff-dev libwebp-dev
```
2. install nvidia cuda
```
sudo apt-key del 7fa2af80 && \
  wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin && \
  sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600 && \
  wget https://developer.download.nvidia.com/compute/cuda/12.3.2/local_installers/cuda-repo-wsl-ubuntu-12-3-local_12.3.2-1_amd64.deb && \
  sudo dpkg -i cuda-repo-wsl-ubuntu-12-3-local_12.3.2-1_amd64.deb && \
  sudo cp /var/cuda-repo-wsl-ubuntu-12-3-local/cuda-*-keyring.gpg /usr/share/keyrings/ && \
  sudo apt-get update && \
  sudo apt-get -y install cuda-toolkit-12-3
```

3. install python numpy
```
curl -L -O https://bootstrap.pypa.io/get-pip.py && \
  python3 get-pip.py && \
  echo 'export PATH="$PATH:$HOME/.local/bin"' >> ~/.bashrc
  pip install numpy
```

4. download and copy nvidia cudnn to cuda directory
```
cp /mnt/c/Users/admin/Downloads/cudnn-linux-x86_64-8.9.7.29_cuda12-archive.tar.xz . && \
  tar -xvf cudnn-linux-x86_64-8.9.7.29_cuda12-archive.tar.xz && \
  sudo cp cudnn-*-archive/include/cudnn*.h /usr/local/cuda-12.3/include && \
  sudo cp -P cudnn-*-archive/lib/libcudnn* /usr/local/cuda-12.3/lib64 && \
  sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda-12.3/lib64/libcudnn*
```

5.
cd /usr/lib/wsl/lib && \
  sudo rm libcuda.so libcuda.so.1 && \
  sudo ln -s libcuda.so.1.1 libcuda.so.1 && \
  sudo ln -s libcuda.so.1 libcuda.so && \
  sudo ldconfig

6. download opencv and opencv-contribute
```
curl -L https://github.com/opencv/opencv/archive/refs/tags/4.9.0.tar.gz | tar xvz && \
  curl -L https://github.com/opencv/opencv_contrib/archive/refs/tags/4.9.0.tar.gz | tar xvz && \
  mkdir -p opencv-4.9.0/build && \
  cd opencv-4.9.0/build
```

7. add path cuda to path
```
echo 'export PATH="$PATH:/usr/local/cuda-12.3/bin"' >> ~/.bashrc && \
  echo 'export LD_LIBRARY_PATH="/usr/local/cuda-12.3/lib64"' >> ~/.bashrc
```

8.
```
cp /mnt/c/Users/admin/Downloads/Video_Codec_SDK_12.1.14.zip . && \
  sudo cp Lib/linux/stubs/x86_64/* /usr/local/cuda-12.3/lib64/ && \
  sudo cp Interface/* /usr/local/cuda-12.3/include/
```

9.
```
cmake \
  -D CMAKE_BUILD_TYPE=RELEASE \
  -D CMAKE_INSTALL_PREFIX=/usr/local \
  -D WITH_CUDA=ON \
  -D WITH_CUDNN=ON \
  -D WITH_CUBLAS=ON \
  -D WITH_TBB=ON \
  -D WITH_NVCUVID=ON \
  -D WITH_NVCUVENC=ON \
  -D OPENCV_DNN_CUDA=ON \
  -D OPENCV_ENABLE_NONFREE=ON \
  -D CUDA_ARCH_BIN=8.9 \
  -D OPENCV_EXTRA_MODULES_PATH=$HOME/python/opencv/opencv_contrib-4.9.0/modules \
  -D BUILD_EXAMPLES=OFF \
  -D HAVE_opencv_python3=ON \
  -D BUILD_opencv_python3=ON \
  -D PYTHON3_EXECUTABLE=$(which python3) \
  -D PYTHON_DEFAULT_EXECUTABLE=$(which python3) \
  -D PYTHON3_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
  -D PYTHON_LIBRARY=$(python3 -c "from distutils.sysconfig import get_config_var;from os.path import dirname,join ; print(join(dirname(get_config_var('LIBPC')),get_config_var('LDLIBRARY')))") \
  -D PYTHON_INCLUDE_DIR=/usr/include/python3.10 \
  -D OPENCV_PYTHON3_INSTALL_PATH=/usr/local/lib/python3.10/dist-packages \
  -D PYTHON3_NUMPY_INCLUDE_DIRS=$HOME/.local/lib/python3.10/site-packages/numpy/core/include \
  ..
```

10.
make -j $(nproc)
sudo make install

# reference
- https://en.wikipedia.org/wiki/CUDA
