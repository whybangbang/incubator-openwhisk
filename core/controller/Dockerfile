# Licensed to the Apache Software Foundation (ASF) under one or more contributor
# license agreements; and to You under the Apache License, Version 2.0.

FROM scala

ENV UID=1001 \
    NOT_ROOT_USER=owuser
ENV SWAGGER_UI_DOWNLOAD_SHA256=3d7ef5ddc59e10f132fe99771498f0f1ba7a2cbfb9585f9863d4191a574c96e7 \
    SWAGGER_UI_VERSION=3.6.0

# Install swagger-ui
RUN curl -sSL -o swagger-ui-v${SWAGGER_UI_VERSION}.tar.gz --no-verbose https://github.com/swagger-api/swagger-ui/archive/v${SWAGGER_UI_VERSION}.tar.gz && \
    echo "${SWAGGER_UI_DOWNLOAD_SHA256}  swagger-ui-v${SWAGGER_UI_VERSION}.tar.gz" | sha256sum -c - && \
    mkdir swagger-ui && \
    tar zxf swagger-ui-v${SWAGGER_UI_VERSION}.tar.gz -C /swagger-ui --strip-components=2 swagger-ui-${SWAGGER_UI_VERSION}/dist && \
    rm swagger-ui-v${SWAGGER_UI_VERSION}.tar.gz && \
    sed -i s#http://petstore.swagger.io/v2/swagger.json#/api/v1/api-docs#g /swagger-ui/index.html

# Copy app jars
ADD build/distributions/controller.tar /

COPY init.sh /
RUN chmod +x init.sh

RUN adduser -D -u ${UID} -h /home/${NOT_ROOT_USER} -s /bin/bash ${NOT_ROOT_USER}
USER ${NOT_ROOT_USER}

EXPOSE 8080
CMD ["./init.sh", "0"]
