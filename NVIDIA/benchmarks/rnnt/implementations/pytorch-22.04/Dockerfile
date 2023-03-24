# Copyright (c) 2019-2022, NVIDIA CORPORATION. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

ARG FROM_IMAGE_NAME=nvcr.io/nvidia/pytorch:22.04-py3
FROM ${FROM_IMAGE_NAME}

ARG http_proxy ${http_proxy}
ARG https_proxy ${https_proxy}
RUN echo "Acquire::http::Proxy \"${http_proxy}\";\nAcquire::https::Proxy \"${https_proxy}\";" | tee /etc/apt/apt.conf.d/80proxy

ENV http_proxy ${http_proxy}
ENV https_proxy ${https_proxy}

COPY ./cuda-keyring_1.0-1_all.deb cuda-keyring_1.0-1_all.deb
RUN dpkg -i cuda-keyring_1.0-1_all.deb

RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
	apt-get install -y vim && \
	apt-get install -y wget && \
	apt-get install -y libsndfile1 git cmake jq && \
    apt-get install -y sox=14.4.2+git20190427-2 && \
    apt-get install -y --no-install-recommends numactl=2.0.12-1 && \
    rm -rf /var/lib/apt/lists/*

# To restore https://github.com/HawkAaron/warp-transducer broken dependency, apply this update:
# https://github.com/pytorch/pytorch/commit/1d681c1ab2e335de2ec32995dc2aa4e57dd873e7#diff-d70a61ff9e3018786ff29ed01db0042e838f23cac5d07c431bc9292e4a5e99deL294

WORKDIR /workspace/rnnt

COPY requirements.txt .
RUN pip install --no-cache --disable-pip-version-check -U -r requirements.txt
RUN pip install ipdb
# RUN pip install tensorboard-tb-profiler

COPY . .
