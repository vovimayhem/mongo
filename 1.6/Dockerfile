# DISCLAIMER:
#
# If your'e seeing this, you'll know this is a VERY OLD (2010-12-08 00:02:29)
# mongodb version that I had to create in order to work on a very old project.
#
# So, PRETTY PLEASE, WITH SUGAR ON TOP, AVOID USING THIS IMAGE unless you really
# know what your'e doing.
#
# I'M NOT MAINTAINING THIS IMAGE for purposes other than the project I'm
# currently working on.
FROM debian:wheezy
MAINTAINER Roberto Quintanilla <roberto.quintanilla@gmail.com>

################################################################################
# 0: Set the MONGO_VERSION we'll install and the download MD5 (I couldn't find
#    the GPG signature!) for this command only.
# 1: Add mongodb user and group first to make sure their IDs get assigned
#    consistently, regardless of whatever dependencies get added.
# 2: Install ca-cerificates & curl.
# 3: grab gosu for easy step-down from root.
# 4: Download, verify & decompress mongodb to /usr/local
# 5: Cleanup of packages installed for the setup.

RUN MONGO_VERSION=1.6.5 MONGO_DOWNLOAD_MD5=0a64adafd9772b6b2d748c3b088bd895 \
	&& groupadd -r mongodb && useradd -r -g mongodb mongodb \
	&& apt-get update \
	&& apt-get install -y --no-install-recommends ca-certificates curl numactl \
	&& rm -rf /var/lib/apt/lists/* \
  && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
	&& curl -o /usr/local/bin/gosu -SL "https://github.com/tianon/gosu/releases/download/1.2/gosu-$(dpkg --print-architecture)" \
	&& curl -o /usr/local/bin/gosu.asc -SL "https://github.com/tianon/gosu/releases/download/1.2/gosu-$(dpkg --print-architecture).asc" \
	&& gpg --verify /usr/local/bin/gosu.asc \
	&& rm /usr/local/bin/gosu.asc \
	&& chmod +x /usr/local/bin/gosu \
  && curl -SL "https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-$MONGO_VERSION.tgz" -o mongo.tgz \
	&& echo "$MONGO_DOWNLOAD_MD5 *mongo.tgz" | md5sum -c - \
	&& tar -xvf mongo.tgz -C /usr/local --strip-components=1 \
	&& rm mongo.tgz* \
	&& apt-get purge -y --auto-remove ca-certificates curl

VOLUME /data/db

COPY docker-entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]

EXPOSE 27017 28017
CMD ["mongod"]
