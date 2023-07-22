FROM alpine:3.18

RUN apk add --no-cache  coreutils=9.3-r1 \ 
                        curl=8.1.2-r0 \ 
                        xz=5.4.3-r0 \
                        jq=1.6-r3 \ 
                        make=4.4.1-r1 \ 
                        qemu-img=8.0.3-r1 && \ 
    adduser --no-create-home --disabled-password --shell /usr/sbin/nologin --uid 10000 runner
USER 10000

WORKDIR /build
ENV OUTPUT_DIR /output

HEALTHCHECK NONE

ENTRYPOINT [ "/usr/bin/make" ]