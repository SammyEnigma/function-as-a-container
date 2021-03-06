FROM mvdan/shfmt as shfmt-builder

FROM matejak/argbash as argbash-builder

FROM donaldrich/builder:latest AS builder
# RUN gget github.com/koalaman/shellcheck shellcheck-*.linux.x86_64.tar.xz --stdout | tar -xJf- shellcheck
RUN ginstall.sh shellcheck latest
RUN upx /usr/local/bin/shellcheck

# FROM donaldrich/builder:latest AS shellharden-builder
RUN gget github.com/anordal/shellharden --executable shellharden='shellharden-*-x86_64-linux-gnu'

# FROM donaldrich/builder:latest AS abs-builder
RUN gget github.com/abs-lang/abs --executable abs='abs-linux-amd64'

# FROM donaldrich/builder:latest AS bat-builder
# RUN gget github.com/sharkdp/bat bat-*-x86_64-unknown-linux-musl.tar.gz --stdout | tar -xzf-
RUN wget -O archive.tar.gz "https://github.com/sharkdp/bat/releases/download/v0.16.0/bat-v0.16.0-x86_64-unknown-linux-musl.tar.gz"
RUN tar xzf archive.tar.gz
RUN mv bat-v0.16.0-x86_64-unknown-linux-musl/bat bat
RUN mv bat /usr/local/bin
RUN chmod a+x /usr/local/bin/bat

# FROM donaldrich/builder:latest AS micro-builder
# RUN gget github.com/zyedidia/micro micro-*-linux64.tar.gz --stdout | tar -xzf- micro

FROM python:alpine

# RUN apk update && apk add bat
RUN pip install --no-cache-dir bashate

COPY --from=builder /usr/local/bin/bat /usr/local/bin/bat
COPY --from=argbash-builder /usr/local/bin/argbash /usr/local/bin/argbash
COPY --from=argbash-builder /usr/local/bin/argbash-1to2 /usr/local/bin/argbash-1to2
COPY --from=argbash-builder /usr/local/bin/argbash-init /usr/local/bin/argbash-init
COPY --from=builder /usr/local/bin/shellcheck /usr/local/bin/shellcheck
COPY --from=builder abs /usr/local/bin/abs
# COPY --from=builder micro /usr/local/bin/micro
COPY --from=builder shellharden /usr/local/bin/shellharden
COPY --from=shfmt-builder /bin/shfmt /usr/local/bin/shfmt
# validate
COPY --from=donaldrich/function:task      /usr/local/bin/tusk /usr/local/bin/tusk
COPY ./Dockerfile ./Dockerfile
COPY ./tusk.yml ./tusk.yml
RUN tusk validate
ENTRYPOINT [ "tusk" ]
