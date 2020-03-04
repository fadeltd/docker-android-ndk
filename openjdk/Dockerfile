ARG ALPINE_VERSION="3.11.3"
ARG ALPINE_IMAGE=alpine:${ALPINE_VERSION}
FROM ${ALPINE_IMAGE}
LABEL maintainer="Fadel Trivandi Dipantara <fadeltd@hotmail.com>"
LABEL description="Alpine 3.11 Open JDK 8"

ENV JAVA_HOME /opt/jdk
ENV PATH $JAVA_HOME/bin:$PATH

# https://adoptopenjdk.net/upstream.html
# > These binaries are built by Red Hat on their infrastructure on behalf of the OpenJDK jdk8u and jdk11u projects.
# > The binaries are created from the unmodified source code at OpenJDK.
# > Although no formal support agreement is provided, please report any bugs you may find to https://bugs.java.com/.
# ARG JAVA_VERSION="8u242"
# ARG JAVA_BUILD_VERSION="b08"
# ARG JAVA_ARCH="x64_linux"
# ARG JAVA_BASE_URL="https://github.com/AdoptOpenJDK/openjdk8-upstream-binaries/releases/download/jdk${JAVA_VERSION}-${JAVA_BUILD_VERSION}"
# ARG JAVA_ASSET="OpenJDK8U-jdk_${JAVA_ARCH}_${JAVA_VERSION}${JAVA_BUILD_VERSION}.tar.gz"
# ARG JAVA_SIGN="${JAVA_ASSET}.sign"
# ARG JAVA_SIGN_URL="${JAVA_BASE_URL}/${JAVA_SIGN}"

# https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html
# Java™ Platform Standard Edition Development Kit (JDK™).
# The JDK is a development environment for building applications, applets, and components using the Java programming language.
# The JDK includes tools useful for developing and testing programs written in the Java programming language and running on the Java platform.
ARG JAVA_VERSION="8u242"
ARG JAVA_BUILD_VERSION="b08"
ARG JAVA_ARCH="linux-x64"
ARG JAVA_HASH="1f5b5a70bf22433b84d0e960903adac8"
ARG JAVA_PACKAGE="server-jre"
ARG JAVA_ASSET="${JAVA_PACKAGE}-${JAVA_VERSION}-${JAVA_ARCH}.tar.gz"
ARG JAVA_BASE_URL="http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}-${JAVA_BUILD_VERSION}/${JAVA_HASH}"
ARG JAVA_ASSET_URL="${JAVA_BASE_URL}/${JAVA_ASSET}"
ARG JAVA_CHECKSUM="https://www.oracle.com/webfolder/s/digest/${JAVA_VERSION}checksum.html"
ARG JAVA_TMP="/tmp/openjdk.tgz"

RUN set -eux; \
    # need gnupg to sign this
	wget -O "${JAVA_TMP}" "--header=Cookie: oraclelicense=accept-securebackup-cookie" "${JAVA_ASSET_URL}" && \
	sha256sum "${JAVA_TMP}"; \
    JAVA_PACKAGE_SHA256=$(curl -sSL "${JAVA_CHECKSUM}" | grep -E "${JAVA_ASSET}" | grep -Eo '(sha256: )[^<]+' | cut -d: -f2 | xargs) && \
	echo "${JAVA_PACKAGE_SHA256} ${JAVA_TMP}" | sha256sum -c && \
	# wget -O "${JAVA_TMP}" "${JAVA_ASSET_URL}"; \
	# wget -O "${JAVA_TMP}.asc" "${JAVA_SIGN_URL}"; \
	mkdir -p "$JAVA_HOME"; \
	tar --extract --file "${JAVA_TMP}" --directory "$JAVA_HOME" --strip-components 1; \
	rm "${JAVA_TMP}"; \
	cd "${JAVA_HOME}/bin"; \
	echo "${PATH}"; \
    ls -lp; \
	chmod +x java; \
	./java --version; \
	# java -Xshare:dump; \
# java smoke test
	java --version; \
	javac --version

# https://docs.oracle.com/javase/10/tools/jshell.htm
# https://docs.oracle.com/javase/10/jshell/
# https://en.wikipedia.org/wiki/JShell
CMD ["jshell"]