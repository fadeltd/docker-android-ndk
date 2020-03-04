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
ARG JAVA_VERSION="8u242"
ARG JAVA_BUILD_VERSION="b08"
ARG JAVA_ARCH="x64_linux"
ARG JAVA_ASSET="OpenJDK8U-jdk_${JAVA_ARCH}_${JAVA_VERSION}${JAVA_BUILD_VERSION}.tar.gz"
# ARG JAVA_SIGN="${JAVA_ASSET}.sign"
ARG JAVA_BASE_URL="https://github.com/AdoptOpenJDK/openjdk8-upstream-binaries/releases/download/jdk${JAVA_VERSION}-${JAVA_BUILD_VERSION}"
ARG JAVA_ASSET_URL="${JAVA_BASE_URL}/${JAVA_ASSET}"
# ARG JAVA_SIGN_URL="${JAVA_BASE_URL}/${JAVA_SIGN}"

RUN set -eux; \
    # need gnupg to sign this
	# wget -O /tmp/openjdk.tgz.asc "${JAVA_SIGN_URL}"; \
	wget -O /tmp/openjdk.tgz "${JAVA_ASSET_URL}"; \
	mkdir -p "$JAVA_HOME"; \
	tar --extract --file /tmp/openjdk.tgz --directory "$JAVA_HOME" --strip-components 1; \
	rm /tmp/openjdk.tgz; \
	cd $JAVA_HOME/bin; \
	echo $PATH; \
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