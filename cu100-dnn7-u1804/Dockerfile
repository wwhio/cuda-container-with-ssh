# cuda 10.0 with cudnn 7 on ubuntu 18.04
# build image with
#   $ docker build -t cuda/100-dnn7-u1804:v1 .
# test image with
#   $ nvidia-docker run --rm -d -p 30001:22 --name cuda10 cuda/100-dnn7-u1804:v1

FROM nvidia/cuda:10.0-cudnn7-devel-ubuntu18.04

# setup ENV , get these ENVs with
#   $ nvidia-docker run --rm -it nvidia/cuda:10.0-cudnn7-devel-ubuntu18.04 printenv
# change the apt source to mirrors.ustc.edu.cn
RUN echo "export CUDA_VERSION=10.0.130" >> /root/.bashrc && \
    echo "export CUDA_PKG_VERSION=10-0=10.0.130-1" >> /root/.bashrc && \
    echo "export CUDNN_VERSION=7.4.2.24" >> /root/.bashrc && \
    echo "export NCCL_VERSION=2.4.2" >> /root/.bashrc && \
    echo "export NVIDIA_DRIVER_CAPABILITIES=compute,utility" >> /root/.bashrc && \
    echo "export NVIDIA_VISIBLE_DEVICES=all" >> /root/.bashrc && \
    echo "export PATH=/usr/local/cuda/bin:$PATH" >> /root/.bashrc && \
    echo "export LIBRARY_PATH=/usr/local/cuda/lib64/stubs" >> /root/.bashrc && \
    mv /etc/apt/sources.list /etc/apt/sources.list.bak && \
    mkdir -p /root/.config/pip/
COPY sources.list /etc/apt/
COPY pip.conf /root/.config/pip/
COPY .condarc /root/

# install openssh
RUN apt-get clean && \
    apt-get update && \
    apt-get install openssh-server -y --no-install-recommends && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    sed -i "s@#PermitRootLogin prohibit-password@PermitRootLogin yes@g" /etc/ssh/sshd_config && \
    mkdir -p /run/sshd && \
    echo "root:login123456" | chpasswd

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
