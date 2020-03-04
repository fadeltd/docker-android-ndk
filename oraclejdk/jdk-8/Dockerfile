ARG ALPINE_VERSION="3.11.3"
ARG ALPINE_IMAGE=alpine:${ALPINE_VERSION}
FROM ${ALPINE_IMAGE}
LABEL maintainer="Fadel Trivandi Dipantara <fadeltd@hotmail.com>"
LABEL description="Alpine 3.11 Open JDK 8"

ENV JAVA_HOME /opt/jdk
ENV PATH $JAVA_HOME/bin:$PATH

# https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html
# Java™ Platform Standard Edition Development Kit (JDK™).
# The JDK is a development environment for building applications, applets, and components using the Java programming language.
# The JDK includes tools useful for developing and testing programs written in the Java programming language and running on the Java platform.
ARG JAVA_VERSION="8u241"
ARG JAVA_BUILD_VERSION="b07"
ARG JAVA_ARCH="linux-x64"
ARG JAVA_HASH="1f5b5a70bf22433b84d0e960903adac8"
ARG JAVA_PACKAGE="server-jre"
ARG JAVA_ASSET="${JAVA_PACKAGE}-${JAVA_VERSION}-${JAVA_ARCH}.tar.gz"
ARG JAVA_BASE_URL="http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}-${JAVA_BUILD_VERSION}/${JAVA_HASH}"
ARG JAVA_ASSET_URL="${JAVA_BASE_URL}/${JAVA_ASSET}"
ARG JAVA_CHECKSUM="https://www.oracle.com/webfolder/s/digest/${JAVA_VERSION}checksum.html"
ARG JAVA_TMP="/tmp/openjdk.tgz"
ARG JAVA_TMP_SHA="${JAVA_TMP}.sha256"

RUN set -eux; \
	apk -U upgrade; \
    apk add --no-cache --virtual .java-deps curl; \
	wget -O "${JAVA_TMP}" "--header=Cookie: oraclelicense=accept-securebackup-cookie" "${JAVA_ASSET_URL}" && \
    sha256sum "${JAVA_TMP}"; \
    JAVA_PACKAGE_SHA256=$(curl -sSL "${JAVA_CHECKSUM}" | grep -E "${JAVA_ASSET}" | grep -Eo '(sha256: )[^<]+' | cut -d: -f2 | xargs) && \
    echo "${JAVA_PACKAGE_SHA256}  ${JAVA_TMP}" | sha256sum -c && \
	# echo "${JAVA_PACKAGE_SHA256}  ${JAVA_TMP}" > "${JAVA_TMP_SHA}"; \
    # sha256sum -c "${JAVA_TMP_SHA}" && \
    mkdir -p "$JAVA_HOME"; \
	tar --extract --file "${JAVA_TMP}" --directory "$JAVA_HOME" --strip-components 1; \
	rm "${JAVA_TMP} ${JAVA_TMP_SHA}"; \
	cd "${JAVA_HOME}/bin"; \
	echo "${PATH}"; \
    ls -lp; \
	chmod +x java; \
	./java --version; \
	# java -Xshare:dump; \
	apk del --no-network .java-deps; \
# java smoke test
	java --version; \
	javac --version

# https://docs.oracle.com/javase/10/tools/jshell.htm
# https://docs.oracle.com/javase/10/jshell/
# https://en.wikipedia.org/wiki/JShell
CMD ["jshell"]
