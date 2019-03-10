# What is TensorFlow Inception?

> The Inception model is a TensorFlow model for image recognition. You can automatically categorize image based on trained data. For more information check [this link](https://www.tensorflow.org/tutorials/image_recognition)

> The TensorFlow Inception docker image allows easily exporting inception data models and querying a TensorFlow server serving the Inception model. For example, it is very easy to start using the already trained data from the ImageNet image database.

[https://www.tensorflow.org/tutorials/image_recognition](https://www.tensorflow.org/tutorials/image_recognition)

# TL;DR;

Before running the docker image you first need to download the Inception model training checkpoint so it will be available for the TensorFlow Serving server.

```bash
$ mkdir /tmp/model-data
$ curl -o '/tmp/model-data/inception-v3-2016-03-01.tar.gz' 'http://download.tensorflow.org/models/image/imagenet/inception-v3-2016-03-01.tar.gz'
$ cd /tmp/model-data
$ tar xzf inception-v3-2016-03-01.tar.gz
```

## Docker Compose

```bash
$ curl -sSL https://raw.githubusercontent.com/bitnami/bitnami-docker-tensorflow-inception/master/docker-compose.yml > docker-compose.yml
$ docker-compose up -d
```

# Why use Bitnami Images?

* Bitnami closely tracks upstream source changes and promptly publishes new versions of this image using our automated systems.
* With Bitnami images the latest bug fixes and features are available as soon as possible.
* Bitnami containers, virtual machines and cloud images use the same components and configuration approach - making it easy to switch between formats based on your project needs.
* All our images are based on [minideb](https://github.com/bitnami/minideb) a minimalist Debian based container image which gives you a small base container image and the familiarity of a leading linux distribution.
* Bitnami container images are released daily with the latest distribution packages available.


> This [CVE scan report](https://quay.io/repository/bitnami/tensorflow-inception?tab=tags) contains a security report with all open CVEs. To get the list of actionable security issues, find the "latest" tag, click the vulnerability report link under the corresponding "Security scan" field and then select the "Only show fixable" filter on the next page.

# How to deploy TensorFlow Inception in Kubernetes?

Deploying Bitnami applications as Helm Charts is the easiest way to get started with our applications on Kubernetes. Read more about the installation in the [Bitnami TensorFlow Inception Chart GitHub repository](https://github.com/bitnami/charts/tree/master/bitnami/tensorflow-inception).

Bitnami containers can be used with [Kubeapps](https://kubeapps.com/) for deployment and management of Helm Charts in clusters.

# Supported tags and respective `Dockerfile` links

> NOTE: Debian 8 images have been deprecated in favor of Debian 9 images. Bitnami will not longer publish new Docker images based on Debian 8.

Learn more about the Bitnami tagging policy and the difference between rolling tags and immutable tags [in our documentation page](https://docs.bitnami.com/containers/how-to/understand-rolling-tags-containers/).


* [`1-ol-7`, `1.11.1-ol-7-r129` (1/ol-7/Dockerfile)](https://github.com/bitnami/bitnami-docker-tensorflow-inception/blob/1.11.1-ol-7-r129/1/ol-7/Dockerfile)
* [`1-debian-9`, `1.11.1-debian-9-r99`, `1`, `1.11.1`, `1.11.1-r99`, `latest` (1/debian-9/Dockerfile)](https://github.com/bitnami/bitnami-docker-tensorflow-inception/blob/1.11.1-debian-9-r99/1/debian-9/Dockerfile)

Subscribe to project updates by watching the [bitnami/tensorflow-inception GitHub repo](https://github.com/bitnami/bitnami-docker-tensorflow-inception).

# Prerequisites

To run this application you need Docker Engine 1.10.0. Docker Compose is recommended with a version 1.6.0 or later.

# How to use this image

## Run TensorFlow Inception client with TensorFlow Serving

Running TensorFlow Inception client with the TensorFlow Serving server is the recommended way. You can either use docker-compose or run the containers manually.

### Run the application using Docker Compose

This is the recommended way to run TensorFlow Inception client. You can use the following `docker-compose.yml` template:

```yaml
version: '2'

services:
  tensorflow-serving:
    image: 'bitnami/tensorflow-serving:latest'
    ports:
      - '8500:8500'
      - '8501:8501'
    volumes:
      - 'tensorflow_serving_data:/bitnami'
      - '/tmp/model-data/:/bitnami/model-data'
  tensorflow-inception:
    image: 'bitnami/tensorflow-inception:latest'
    volumes:
      - 'tensorflow_inception_data:/bitnami'
      - '/tmp/model-data/:/bitnami/model-data'
    depends_on:
      - tensorflow-serving

volumes:
  tensorflow_serving_data:
    driver: local
  tensorflow_inception_data:
    driver: local
```

### Run the application manually

If you want to run the application manually instead of using docker-compose, these are the basic steps you need to run:

1. Create a new network for the application and the database:

  ```bash
  $ docker network create tensorflow-tier
  ```

2. Start a Tensorflow Serving server in the network generated:

  ```bash
  $ docker run -d -v /tmp/model-data:/bitnami/model-data -p 8500:8500 -p 8501:8501 --name tensorflow-serving --net tensorflow-tier bitnami/tensorflow-serving:latest
  ```

  *Note:* You need to give the container a name in order to TensorFlow Inception client to resolve the host

3. Run the TensorFlow Inception client container:

  ```bash
  $ docker run -d -v /tmp/model-data:/bitnami/model-data --name tensorflow-inception --net tensorflow-tier bitnami/tensorflow-inception:latest
  ```

## Persisting your application

If you remove the container all your data and configurations will be lost, and the next time you run the image the database will be reinitialized. To avoid this loss of data, you should mount a volume that will persist even after the container is removed.

For persistence you should mount a volume at the `/bitnami` path. Additionally you should mount a volume for [persistence of the TensorFlow Serving configuration](https://github.com/bitnami/bitnami-docker-tensorflow-serving#persisting-your-configuration).

The above examples define docker volumes namely `tensorflow_serving_data` and `tensorflow_inception_data`. The ensorFlow Inception client application state will persist as long as these volumes are not removed.

To avoid inadvertent removal of these volumes you can [mount host directories as data volumes](https://docs.docker.com/engine/tutorials/dockervolumes/). Alternatively you can make use of volume plugins to host the volume data.

### Mount host directories as data volumes with Docker Compose

This requires a minor change to the `docker-compose.yml` template previously shown:

```yaml
version: '2'

services:
  tensorflow-serving:
    image: 'bitnami/tensorflow-serving:latest'
    ports:
      - '8500:8500'
      - '8501:8501'
    volumes:
      - '/path/to/tensorflow-serving-persistence:/bitnami'

  tensorflow-inception:
    image: 'bitnami/tensorflow-inception:latest'
    depends_on:
      - tensorflow-serving
    volumes:
      - '/path/to/tensorflow-inception-persistence:/bitnami'
```

### Mount host directories as data volumes using the Docker command line

1. Create a network (if it does not exist):

  ```bash
  $ docker network create tensorflow-tier
  ```

2. Create a Tensorflow-Serving container with host volume:

  ```bash
  $ docker run -d --name tensorflow-serving -p 8500:8500 -p 8501:8501 \
    --net tensorflow-tier \
    --volume /path/to/tensorflow-serving-persistence:/bitnami \
    --volume /path/to/model_data:/bitnami/model-data \
    bitnami/tensorflow-serving:latest
  ```

  *Note:* You need to give the container a name in order to TensorFlow Inception client to resolve the host

3. Create the TensorFlow Inception client container with host volumes:

  ```bash
  $ docker run -d --name tensorflow-inception \
    --net tensorflow-tier \
    --volume /path/to/tensorflow-inception-persistence:/bitnami \
    --volume /path/to/model_data:/bitnami/model-data \
    bitnami/tensorflow-inception:latest
  ```

# Upgrade this application

Bitnami provides up-to-date versions of Tensorflow-Serving and TensorFlow Inception client, including security patches, soon after they are made upstream. We recommend that you follow these steps to upgrade your container. We will cover here the upgrade of the TensorFlow Inception client container. For the Tensorflow-Serving upgrade see https://github.com/bitnami/bitnami-docker-tensorflow-serving/blob/master/README.md#upgrade-this-image

1. Get the updated images:

  ```bash
  $ docker pull bitnami/tensorflow-inception:latest
  ```

2. Stop your container

 * For docker-compose: `$ docker-compose stop tensorflow-inception`
 * For manual execution: `$ docker stop tensorflow-inception`

3. Take a snapshot of the application state

```console
$ rsync -a tensorflow-inception-persistence tensorflow-inception-persistence.bkp.$(date +%Y%m%d-%H.%M.%S)
```

Additionally, [snapshot the TensorFlow Serving data](https://github.com/bitnami/bitnami-docker-mariadb#step-2-stop-and-backup-the-currently-running-container)

You can use these snapshots to restore the application state should the upgrade fail.

4. Remove the currently running container

 * For docker-compose: `$ docker-compose rm tensorflow-inception`
 * For manual execution: `$ docker rm tensorflow-inception`

5. Run the new image

 * For docker-compose: `$ docker-compose up tensorflow-inception`
 * For manual execution ([mount](#mount-persistent-folders-manually) the directories if needed): `docker run --name tensorflow-inception bitnami/tensorflow-inception:latest`

# Configuration

## Environment variables

When you start the tensorflow-inception image, you can adjust the configuration of the instance by passing one or more environment variables either on the docker-compose file or on the docker run command line. If you want to add a new environment variable:

 * For docker-compose add the variable name and value under the application section:

```yaml
tensorflow-inception:
  image: bitnami/tensorflow-inception:latest
  environment:
    - TENSORFLOW_INCEPTION_MODEL_INPUT_DATA_NAME=my_custom_data
  volumes_from:
    - tensorflow_inception_data
```

 * For manual execution add a `-e` option with each variable and value:

  ```bash
  $ docker run -d --name tensorflow-inception \
    --net tensorflow-tier \
    --volume /path/to/tensorflow-inception-persistence:/bitnami \
    bitnami/tensorflow-inception:latest
  ```

Available variables:

 - `TENSORFLOW_SERVING_HOST`: Hostname for Tensorflow-Serving server. Default: **tensorflow-serving**
 - `TENSORFLOW_SERVING_PORT_NUMBER`: Port used by Tensorflow-Serving server. Default: **8500**
 - `TENSORFLOW_INCEPTION_MODEL_INPUT_DATA_NAME`: Folder containing the data model to export. Default: **inception-v3**

# Contributing

We'd love for you to contribute to this container. You can request new features by creating an [issue](https://github.com/bitnami/bitnami-docker-tensorflow-inception/issues), or submit a [pull request](https://github.com/bitnami/bitnami-docker-tensorflow-inception/pulls) with your contribution.

# Issues

If you encountered a problem running this container, you can file an [issue](https://github.com/bitnami/bitnami-docker-tensorflow-inception/issues). For us to provide better support, be sure to include the following information in your issue:

- Host OS and version
- Docker version (`$ docker version`)
- Output of `$ docker info`
- Version of this container (`$ echo $BITNAMI_IMAGE_VERSION` inside the container)
- The command you used to run the container, and any relevant output you saw (masking any sensitive information)

# License

Copyright (c) 2019 Bitnami

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

  <http://www.apache.org/licenses/LICENSE-2.0>

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
