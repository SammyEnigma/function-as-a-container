FROM docker:latest as docker

FROM donaldrich/builder:latest AS builder
RUN wget "https://github.com/aelsabbahy/goss/releases/latest/download/goss-linux-amd64" -O /usr/local/bin/goss
RUN chmod +rx /usr/local/bin/goss
RUN upx /usr/local/bin/goss

RUN wget "https://github.com/aelsabbahy/goss/releases/latest/download/dgoss" -O /usr/local/bin/dgoss
RUN chmod +rx /usr/local/bin/dgoss
RUN upx /usr/local/bin/dgoss

RUN wget "https://raw.githubusercontent.com/aelsabbahy/goss/master/extras/dcgoss/dcgoss" -O /usr/local/bin/dcgoss
RUN chmod +rx /usr/local/bin/dcgoss

RUN wget "https://raw.githubusercontent.com/aelsabbahy/goss/master/extras/kgoss/kgoss" -O /usr/local/bin/kgoss
RUN chmod +rx /usr/local/bin/kgoss

FROM donaldrich/builder:latest AS container-structure-test-builder
RUN wget "https://storage.googleapis.com/container-structure-test/latest/container-structure-test-linux-amd64" -O /usr/local/bin/container-structure-test
RUN chmod +rx /usr/local/bin/container-structure-test
RUN upx /usr/local/bin/container-structure-test

FROM donaldrich/builder:latest AS inspec-builder
RUN curl https://omnitruck.chef.io/install.sh | bash -s -- -P inspec

# FROM donaldrich/builder:latest AS converge-builder
# RUN curl get.converge.sh | bash -

FROM donaldrich/builder:go AS converge-builder
# RUN gget github.com/asteris-llc/converge '*linux_amd64.tar.gz'
RUN wget -O archive.tar.gz "https://github.com/asteris-llc/converge/releases/download/0.6.0/converge_0.6.0_linux_amd64.tar.gz"
RUN tar xzf archive.tar.gz
RUN upx converge
RUN mv converge /usr/local/bin
RUN chmod a+x /usr/local/bin/converge

FROM alpine

RUN apk add --no-cache bash

COPY --from=converge-builder /usr/local/bin/converge /usr/local/bin/converge
COPY --from=container-structure-test-builder /usr/local/bin/container-structure-test /usr/local/bin/container-structure-test
COPY --from=docker  /usr/local/bin/docker  /usr/local/bin/docker
COPY --from=builder /usr/local/bin/dgoss /usr/local/bin/dgoss
COPY --from=builder /usr/local/bin/goss /usr/local/bin/goss
COPY --from=builder /usr/local/bin/dcgoss /usr/local/bin/dcgoss
COPY --from=builder /usr/local/bin/kgoss /usr/local/bin/kgoss
# validate
COPY --from=donaldrich/function:task      /usr/local/bin/tusk /usr/local/bin/tusk
COPY ./Dockerfile ./Dockerfile
COPY ./tusk.yml ./tusk.yml
RUN tusk validate
ENTRYPOINT [ "tusk" ]
