FROM bitnami/oraclelinux-extras:7-r308
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV PATH="/opt/bitnami/python/bin:/opt/bitnami/tensorflow-inception/bazel-bin/tensorflow_serving/example:$PATH"

# Install required system packages and dependencies
RUN install_packages bsdtar bzip2-libs glibc keyutils-libs krb5-libs libcom_err libgcc libselinux libstdc++ ncurses-libs nss-softokn-freebl openssl-libs pcre readline sqlite zlib
RUN bitnami-pkg install python-2.7.16-0 --checksum f1818dfd2f72ba498d54702d4e6aa44fcdecc1f0b1fc7d99703e74b4626f6395
RUN bitnami-pkg unpack tensorflow-inception-1.11.1-20 --checksum 456b7fa582223eaa971aa9fecd7d882f31aebca1d8f323cb33b323b62bea02aa
RUN pip install --upgrade pip && \
    pip install enum34 futures mock numpy backports.weakref && \
    pip install Keras_Applications==1.0.5 Keras_Preprocessing==1.0.3 --no-deps && \
    pip install -i https://testpypi.python.org/simple --pre grpcio

COPY rootfs /
ENV BITNAMI_APP_NAME="tensorflow-inception" \
    BITNAMI_IMAGE_VERSION="1.11.1-ol-7-r152" \
    TENSORFLOW_INCEPTION_MODEL_INPUT_DATA_NAME="inception-v3" \
    TENSORFLOW_SERVING_HOST="tensorflow-serving" \
    TENSORFLOW_SERVING_PORT_NUMBER="8500"

ENTRYPOINT [ "/app-entrypoint.sh" ]
CMD [ "tail", "-f", "/dev/null" ]
