# cuda 8.0 with cudnn 6 on ubuntu 16.04
# build image with
#   $ docker build -t cuda/80-dnn6-u1604:v1 .
# test image with
#   $ nvidia-docker run --rm -d -p 30001:22 --name cuda80dnn6 cuda/80-dnn6-u1604:v1

FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04

# setup ENV , get these ENVs with
#   $ nvidia-docker run --rm -it nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04 printenv
RUN echo "export CUDA_VERSION=8.0.61" >> /root/.bashrc && \
    echo "export CUDA_PKG_VERSION=8-0=8.0.61-1" >> /root/.bashrc && \
    echo "export CUDNN_VERSION=6.0.21" >> /root/.bashrc && \
    echo "export NVIDIA_DRIVER_CAPABILITIES=compute,utility" >> /root/.bashrc && \
    echo "export NVIDIA_VISIBLE_DEVICES=all" >> /root/.bashrc && \
    echo "export PATH=/usr/local/nvidia/bin:/usr/local/cuda/bin:$PATH" >> /root/.bashrc && \
    echo "export LIBRARY_PATH=/usr/local/cuda/lib64/stubs" >> /root/.bashrc && \
    echo "export LD_LIBRARY_PATH=/usr/local/nvidia/lib:/usr/local/nvidia/lib64" >> /root/.bashrc

# install openssh
RUN apt-get clean && \
    apt-get update && \
    apt-get install openssh-server -y --no-install-recommends && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    sed -i "s@PermitRootLogin prohibit-password@PermitRootLogin yes@g" /etc/ssh/sshd_config && \
    mkdir -p /run/sshd && \
    echo "root:login123456" | chpasswd

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
