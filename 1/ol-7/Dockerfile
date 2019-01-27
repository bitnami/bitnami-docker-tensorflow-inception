FROM bitnami/oraclelinux-extras:7-r242
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV PATH="/opt/bitnami/python/bin:/opt/bitnami/tensorflow-inception/bazel-bin/tensorflow_serving/example:$PATH"

# Install required system packages and dependencies
RUN install_packages bsdtar bzip2-libs glibc keyutils-libs krb5-libs libcom_err libgcc libselinux libstdc++ ncurses-libs nss-softokn-freebl openssl-libs pcre readline sqlite zlib
RUN bitnami-pkg install python-2.7.15-20 --checksum 74fe4964d24180d043f2ee328097a414c69ac7c955cfad228057bbeb9b4ab96f
RUN bitnami-pkg unpack tensorflow-inception-1.11.1-20 --checksum 456b7fa582223eaa971aa9fecd7d882f31aebca1d8f323cb33b323b62bea02aa
RUN pip install --upgrade pip && \
    pip install enum34 futures mock numpy backports.weakref && \
    pip install Keras_Applications==1.0.5 Keras_Preprocessing==1.0.3 --no-deps && \
    pip install -i https://testpypi.python.org/simple --pre grpcio
    

COPY rootfs /
ENV BITNAMI_APP_NAME="tensorflow-inception" \
    BITNAMI_IMAGE_VERSION="1.11.1-ol-7-r98" \
    TENSORFLOW_INCEPTION_MODEL_INPUT_DATA_NAME="inception-v3" \
    TENSORFLOW_SERVING_HOST="tensorflow-serving" \
    TENSORFLOW_SERVING_PORT_NUMBER="8500"

ENTRYPOINT [ "/app-entrypoint.sh" ]
CMD [ "tail", "-f", "/dev/null" ]
