FROM bitnami/minideb-extras:jessie-r28
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV PATH="/opt/bitnami/python/bin:/opt/bitnami/tensorflow-inception/bazel-bin/tensorflow_serving/example:$PATH"

# Install required system packages and dependencies
RUN install_packages libbz2-1.0 libc6 libgcc1 libncurses5 libreadline6 libsqlite3-0 libssl1.0.0 libstdc++6 libtinfo5 zlib1g
RUN bitnami-pkg install python-2.7.14-1 --checksum 8ffb7eaa43bc85a31b8bb9d844332c0f7e3e64e0578cd3ecddabc87ab3695c1e
RUN bitnami-pkg unpack tensorflow-inception-1.5.0-0 --checksum 571532f7ef73bf27d05cef63f099aa5eb9cd4e2fc3dae8cd0607a37dc5edd42d
RUN pip install --upgrade pip && \
    pip install enum34 futures mock numpy backports.weakref && \
    pip install -i https://testpypi.python.org/simple --pre grpcio
    

COPY rootfs /
ENV BITNAMI_APP_NAME="tensorflow-inception" \
    BITNAMI_IMAGE_VERSION="1.5.0-r5" \
    TENSORFLOW_INCEPTION_MODEL_INPUT_DATA_NAME="inception-v3" \
    TENSORFLOW_SERVING_HOST="tensorflow-serving" \
    TENSORFLOW_SERVING_PORT_NUMBER="9000"

ENTRYPOINT ["/app-entrypoint.sh"]
CMD ["tail","-f","/dev/null"]
