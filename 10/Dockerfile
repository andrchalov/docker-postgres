
FROM postgres:10.1

RUN localedef -i ru_RU -c -f UTF-8 -A /usr/share/locale/locale.alias ru_RU.UTF-8

ENV \
  LANG=ru_RU.utf8 \
	PGTAP_VERSION=0.98.0 \
  URL_ENCODE_VERSION=1.2.3

RUN \
  apt-get update && apt-get -y install \
  build-essential \
  wget

RUN \
  apt-get -y install \
  postgresql-server-dev-10 \
  postgresql-plpython-10

# Install pgtap
RUN \
  wget https://github.com/theory/pgtap/archive/v${PGTAP_VERSION}.tar.gz && \
  tar -xzvf v${PGTAP_VERSION}.tar.gz && \
  rm -f v${PGTAP_VERSION}.tar.gz && \
  cd pgtap-${PGTAP_VERSION} && \
  make && \
  make install && \
  cd .. && \
  rm -rf pgtap-${PGTAP_VERSION}

# Install url_encode
RUN \
  wget https://github.com/okbob/url_encode/archive/v${URL_ENCODE_VERSION}.tar.gz && \
  tar -xzf v${URL_ENCODE_VERSION}.tar.gz && \
  cd url_encode-${URL_ENCODE_VERSION} && \
  make && \
  make install && \
  cd .. && \
  rm -Rdf url_encode-${URL_ENCODE_VERSION} v${URL_ENCODE_VERSION}.tar.gz

RUN \
  apt-get -y remove postgresql-server-dev-10 && \
  apt-get clean

COPY docker-entrypoint.sh /
RUN chmod 755 /docker-entrypoint.sh

ENTRYPOINT ["/docker-entrypoint.sh"]

EXPOSE 5432

CMD ["postgres"]
