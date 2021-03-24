FROM golang:1.13.4 AS go-builder

WORKDIR /go/src/github.com/terra-project/core

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install libusb-dev liblz4-tool aria2 jq -y
RUN git clone --recursive ${UPSTREAM_VERSION} https://www.github.com/terra-project/core.git .

RUN make install
ENV TERRAD_HOME=/.terrad
WORKDIR $TERRAD_HOME

EXPOSE 26656 26657 26658
EXPOSE 1317

COPY ./scripts/entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod u+x /usr/local/bin/entrypoint.sh
ENTRYPOINT [ "/usr/local/bin/entrypoint.sh" ]

# STOPSIGNAL SIGINT
