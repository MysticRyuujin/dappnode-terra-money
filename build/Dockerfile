FROM golang:1.13.4 AS go-builder

WORKDIR /go/src/github.com/terra-project/core

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install libusb-dev -y
RUN git clone --recursive ${UPSTREAM_VERSION} https://www.github.com/terra-project/core.git .

RUN make install


# Final image
FROM golang:1.13.4


# Install ca-certificates
#RUN apt-get update && apt-get install ca-certificates supervisor lz4

# Temp directory for copying binaries
WORKDIR /go/src/github.com/terra-project/core

# Copy over binaries from the build-env
COPY --from=go-builder /go/src/github.com/terra-project/core /go/src/github.com/terra-project/core
COPY --from=go-builder /go /go
# Remove temp files

# Add supervisor configuration files
RUN mkdir -p /etc/supervisor/conf.d/
COPY /supervisor/supervisord.conf /etc/supervisor/supervisord.conf
COPY /supervisor/conf.d/* /etc/supervisor/conf.d/

ENV TERRAD_HOME=/.terrad
WORKDIR $TERRAD_HOME

EXPOSE 26656 26657 26658
EXPOSE 1317

# VOLUME [ /.terrad ]

COPY ./scripts/entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod u+x /usr/local/bin/entrypoint.sh
ENTRYPOINT [ "/usr/local/bin/entrypoint.sh" ]

# STOPSIGNAL SIGINT
