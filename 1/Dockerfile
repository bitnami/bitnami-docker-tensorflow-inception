FROM bitnami/minideb-extras:jessie-r22
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV PATH="/opt/bitnami/python/bin:/opt/bitnami/tensorflow-inception/bazel-bin/tensorflow_serving/example:$PATH"

# Install required system packages and dependencies
RUN install_packages libbz2-1.0 libc6 libgcc1 libncurses5 libreadline6 libsqlite3-0 libssl1.0.0 libstdc++6 libtinfo5 zlib1g
RUN bitnami-pkg install python-2.7.14-0 --checksum 2ad513d9d4d4e2c2f98d96448617b5ab3c5dd48dc2e3cb02080ecc7215039ffa
RUN bitnami-pkg unpack tensorflow-inception-1.4.0-0 --checksum e2b24f344dc98fe6e91dc19c6a46c264a36e93301c4be66ca4566560dec8937a
RUN pip install --upgrade pip && \
    pip install enum34 futures mock numpy backports.weakref && \
    pip install -i https://testpypi.python.org/simple --pre grpcio
    

COPY rootfs /

ENV BITNAMI_APP_NAME="tensorflow-inception" \
    BITNAMI_IMAGE_VERSION="1.4.0-r0" \
    TENSORFLOW_INCEPTION_MODEL_INPUT_DATA_NAME="inception-v3" \
    TENSORFLOW_SERVING_HOST="tensorflow-serving" \
    TENSORFLOW_SERVING_PORT_NUMBER="9000"

ENTRYPOINT ["/app-entrypoint.sh"]
CMD ["tail","-f","/dev/null"]
