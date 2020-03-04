ARG ALPINE_VERSION="3.11.3"
ARG ALPINE_IMAGE=alpine:${ALPINE_VERSION}
FROM ${ALPINE_IMAGE}
LABEL maintainer="Fadel Trivandi Dipantara <fadeltd@hotmail.com>"
LABEL description="Alpine 3.11 Open JDK 8"

ENV LANG C.UTF-8

# Use shell script to get JAVA_HOME value
# based on JDK or JRE is installed
RUN { \
		echo '#!/bin/sh'; \
		echo 'set -eux'; \
		echo; \
		echo 'dirname "$(dirname "$(readlink -f "$(which javac || which java)")")"'; \
	} > /usr/local/bin/docker-java-home && \
	chmod +x /usr/local/bin/docker-java-home

ARG JAVA_MAJOR_VERSION="8"
ARG JAVA_MINOR_VERSION="242"
ARG JAVA_BUILD_VERSION="08"
ARG JAVA_VERSION="${JAVA_MAJOR_VERSION}u${JAVA_MINOR_VERSION}"
ARG JAVA_ALPINE_VERSION="${JAVA_MAJOR_VERSION}.${JAVA_MINOR_VERSION}.${JAVA_BUILD_VERSION}-r0"

ENV JAVA_VERSION ${JAVA_VERSION} \
	JAVA_HOME /usr/lib/jvm/java-1.${JAVA_MAJOR_VERSION}.0_${JAVA_MINOR_VERSION} \
	PATH $PATH:${JAVA_HOME}/jre/bin:${JAVA_HOME}/bin

# https://pkgs.alpinelinux.org/package/v3.11/community/x86/openjdk8
RUN set -eux && \
	apk add --no-cache openjdk8="${JAVA_ALPINE_VERSION}" && \
	[ "$JAVA_HOME" = "$(docker-java-home)" ]
