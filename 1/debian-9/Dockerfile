FROM bitnami/minideb-extras:stretch-r106
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV PATH="/opt/bitnami/python/bin:/opt/bitnami/tensorflow-inception/bazel-bin/tensorflow_serving/example:$PATH"

# Install required system packages and dependencies
RUN install_packages libarchive-tools libbz2-1.0 libc6 libgcc1 libncurses5 libreadline7 libsqlite3-0 libssl1.1 libstdc++6 libtinfo5 zlib1g
RUN bitnami-pkg install python-2.7.15-2 --checksum 1d5e9d3a2f7225d159913e3a8792577057abc0ceba7c7d60299bb8d59933258c
RUN bitnami-pkg unpack tensorflow-inception-1.9.1-0 --checksum d29ac4c3eeb69f748a9e5f404c51f127c376dc5bfb4a54ea213ec827cee54fee
RUN pip install --upgrade pip && \
    pip install enum34 futures mock numpy backports.weakref && \
    pip install -i https://testpypi.python.org/simple --pre grpcio
    

COPY rootfs /
ENV BITNAMI_APP_NAME="tensorflow-inception" \
    BITNAMI_IMAGE_VERSION="1.9.1-debian-9-r5" \
    TENSORFLOW_INCEPTION_MODEL_INPUT_DATA_NAME="inception-v3" \
    TENSORFLOW_SERVING_HOST="tensorflow-serving" \
    TENSORFLOW_SERVING_PORT_NUMBER="8500"

ENTRYPOINT ["/app-entrypoint.sh"]
CMD ["tail","-f","/dev/null"]
