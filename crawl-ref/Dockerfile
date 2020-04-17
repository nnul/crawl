FROM ubuntu:18.04 as build

RUN apt-get update && \
	apt-get install -y make g++ git \
	build-essential libncursesw5-dev bison flex liblua5.1-0-dev \
	libsqlite3-dev libz-dev pkg-config python-yaml binutils-gold

WORKDIR /var/tmp/build
COPY source/dat ./dat/
COPY source/debian ./debian/
COPY source/rltiles ./rltiles/
COPY source/scripts ./scripts/
COPY source/util ./util/
COPY source/art-data.txt ./
COPY source/*.cc ./
COPY source/*.h ./
COPY source/Makefile ./
COPY source/Makefile.obj ./
RUN make -j4 NO_DOCS=1

FROM ubuntu:18.04

RUN apt-get update && \
	apt-get install -y liblua5.1-0 libsqlite3-0

# Add user
RUN groupadd -g 2001 crawl \
	&& useradd -m -u 2001 -g crawl crawl
USER crawl

WORKDIR /opt/crawl
RUN mkdir saves
RUN mkdir morgue
COPY --from=build --chown=1000 /var/tmp/build/crawl ./
COPY --from=build --chown=1000 /var/tmp/build/dat ./dat/

ENTRYPOINT ["./crawl"]
