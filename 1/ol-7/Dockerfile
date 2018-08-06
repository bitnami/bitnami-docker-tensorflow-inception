FROM bitnami/oraclelinux-extras:7-r60
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV PATH="/opt/bitnami/python/bin:/opt/bitnami/tensorflow-inception/bazel-bin/tensorflow_serving/example:$PATH"

# Install required system packages and dependencies
RUN install_packages bsdtar bzip2-libs glibc keyutils-libs krb5-libs libcom_err libgcc libselinux libstdc++ ncurses-libs nss-softokn-freebl openssl-libs pcre readline sqlite zlib
RUN bitnami-pkg install python-2.7.15-2 --checksum 3dfc1b08e44ec28473c662fac58bd599ef8ea961f7571057efae7440718cb42e
RUN bitnami-pkg unpack tensorflow-inception-1.9.1-0 --checksum 4212cf59882d2b7a6adb9d2d9ca14ee4eb4b4747ad6bcc5062311eda59923d6e
RUN pip install --upgrade pip && \
    pip install enum34 futures mock numpy backports.weakref && \
    pip install -i https://testpypi.python.org/simple --pre grpcio
    

COPY rootfs /
ENV BITNAMI_APP_NAME="tensorflow-inception" \
    BITNAMI_IMAGE_VERSION="1.9.1-ol-7-r0" \
    TENSORFLOW_INCEPTION_MODEL_INPUT_DATA_NAME="inception-v3" \
    TENSORFLOW_SERVING_HOST="tensorflow-serving" \
    TENSORFLOW_SERVING_PORT_NUMBER="8500"

ENTRYPOINT ["/app-entrypoint.sh"]
CMD ["tail","-f","/dev/null"]
