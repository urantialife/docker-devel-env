ARG BASE_IMAGE
FROM $BASE_IMAGE

LABEL maintainer="Bernd Doser <bernd.doser@braintwister.eu>"

ARG VERSION

RUN ln -snf /usr/bin/python3 /usr/bin/python

RUN apt-get update \
 && apt-get install -y \
    python3-tk \
    unzip \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

RUN pip install \
    graphviz \
    matplotlib \
    mock \
    numpy \
    pydot \
    seaborn \
    setuptools \
    six \
    sklearn \
    stn \
    wheel

RUN pip install --no-deps \
    keras_applications==1.0.6 \
    keras_preprocessing==1.0.5

ARG BAZEL_VERSION=3.4.1
RUN wget -q https://github.com/bazelbuild/bazel/releases/download/$BAZEL_VERSION/bazel-$BAZEL_VERSION-installer-linux-x86_64.sh \
 && chmod +x bazel-$BAZEL_VERSION-installer-linux-x86_64.sh \
 && ./bazel-$BAZEL_VERSION-installer-linux-x86_64.sh

SHELL ["/bin/bash", "-c"]
RUN source /usr/local/lib/bazel/bin/bazel-complete.bash

ENV TF_NEED_CUDA=1
#    TF_CUDA_VERSION=${CUDA_VERSION%.*} \
#    TF_CUDNN_VERSION=${CUDNN_VERSION%.*.*}

# Hack to find cuda libraries
RUN ln -s /usr/lib/x86_64-linux-gnu/libcublas.so.10 /usr/local/cuda/lib64/libcublas.so.10

RUN git clone https://github.com/tensorflow/tensorflow \
 && cd tensorflow \
 && git checkout v$VERSION \
 && ./configure \
 && bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package \
 && ./bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg \
 && pip install /tmp/tensorflow_pkg/tensorflow-$VERSION-cp36-cp36m-linux_x86_64.whl \
 && rm -r /tensorflow /tmp/tensorflow_pkg /bazel-0.25.2-installer-linux-x86_64.sh /root/.cache/bazel
