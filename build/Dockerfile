FROM debian:bullseye-slim as binary

ARG VERSION=0.21.1

ENV FILENAME bitcoin-${VERSION}-x86_64-linux-gnu.tar.gz
ENV DOWNLOAD_URL https://bitcoin.org/bin/bitcoin-core-${VERSION}/${FILENAME}
ENV SHA256SUM 366eb44a7a0aa5bd342deea215ec19a184a11f2ca22220304ebb20b9c8917e2b

ADD $DOWNLOAD_URL /$FILENAME
RUN if [ x"$( sha256sum /${FILENAME} | awk '{print $1}' )" != x"${SHA256SUM}" ]; then \
  rm -f /$FILENAME; \
  echo "Checksum verification failed."; \
  exit 1; \
  else \
  tar xzvf /$FILENAME; \
  mv /bitcoin-${VERSION} /bitcoin; \
  fi

FROM debian:bullseye-slim AS final

EXPOSE 8332 8333 18332 18333 28332 28333

COPY --from=binary /bitcoin/bin/bitcoind /usr/local/bin/

ADD ./bin/docker_entrypoint.sh /usr/local/bin/docker_entrypoint.sh
RUN chmod a+x /usr/local/bin/docker_entrypoint.sh

ENTRYPOINT ["/usr/local/bin/docker_entrypoint.sh"]
