ARG ALPINE_VERSION="3.11.3"
ARG ALPINE_IMAGE=alpine:${ALPINE_VERSION}

FROM ${ALPINE_IMAGE}

LABEL maintainer="Fadel Trivandi Dipantara <fadeltd@hotmail.com>"
LABEL description="Alpine 3.11 Oracle JDK 8"

# https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html
# Java™ Platform Standard Edition Development Kit (JDK™).
# The JDK is a development environment for building applications, applets, and components using the Java programming language.
# The JDK includes tools useful for developing and testing programs written in the Java programming language and running on the Java platform.
ARG JAVA_MAJOR_VERSION="8"
ARG JAVA_MINOR_VERSION="241"
ARG JAVA_BUILD_VERSION="b07"
ARG JAVA_VERSION="${JAVA_MAJOR_VERSION}u${JAVA_MINOR_VERSION}"
ARG JAVA_ARCH="linux-x64"
# ARG JAVA_ORACLE_AUTH="1f5b5a70bf22433b84d0e960903adac8"
# ARG JAVA_PACKAGE="server-jre"
# ARG JAVA_BASE_URL="http://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}-${JAVA_BUILD_VERSION}/${JAVA_ORACLE_AUTH}"
ARG JAVA_ORACLE_AUTH="1f5b5a70bf22433b84d0e960903adac8"
ARG JAVA_PACKAGE="jdk"
ARG JAVA_BASE_URL="https://download.oracle.com/otn-pub/java/jdk/${JAVA_VERSION}-${JAVA_BUILD_VERSION}/${JAVA_ORACLE_AUTH}"
ARG JAVA_ASSET="${JAVA_PACKAGE}-${JAVA_VERSION}-${JAVA_ARCH}.tar.gz"
ARG JAVA_ASSET_URL="${JAVA_BASE_URL}/${JAVA_ASSET}"
ARG JAVA_CHECKSUM="https://www.oracle.com/webfolder/s/digest/${JAVA_VERSION}checksum.html"
ARG JAVA_TMP="/tmp/oracle-jdk.tgz"

ARG JAVA_JVM_PATH="/usr/lib/jvm"
ARG JAVA_JDK="java-1.8-oracle"
ENV JAVA_HOME "${JAVA_JVM_PATH}/${JAVA_JDK}"
ENV PATH $PATH:$JAVA_HOME/bin
# ENV PATH $PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin

ARG GLIBC_VERSION="2.30-r0"
ARG GLIBC_REPO="https://github.com/sgerrand/alpine-pkg-glibc"
ARG GLIBC_DOWNLOAD="${GLIBC_REPO}/releases/download/${GLIBC_VERSION}"
ARG GLIBC_PACKAGES="glibc-${GLIBC_VERSION}.apk glibc-bin-${GLIBC_VERSION}.apk glibc-i18n-${GLIBC_VERSION}.apk"
ARG GLIBC_PATH="/usr/glibc-compat"

RUN set -eux; \
	apk -U upgrade; \
    apk add --no-cache --virtual .java-deps libstdc++ curl; \
# Required glibc dependencies
	for pkg in ${GLIBC_PACKAGES}; do wget -O "/tmp/${pkg}" "${GLIBC_DOWNLOAD}/${pkg}"; done && \
	apk add --allow-untrusted /tmp/*.apk && \
    rm -v /tmp/*.apk; \
    ${GLIBC_PATH}/bin/localedef --force --inputfile POSIX --charmap UTF-8 C.UTF-8; \
    echo "export LANG=C.UTF-8" > /etc/profile.d/locale.sh; \
    ${GLIBC_PATH}/sbin/ldconfig /lib ${GLIBC_PATH}/lib; \
    echo 'hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4' >> /etc/nsswitch.conf; \
# Install java from oracle
    wget -O "${JAVA_TMP}" "--header=Cookie: oraclelicense=accept-securebackup-cookie" "${JAVA_ASSET_URL}" && \
	sha256sum "${JAVA_TMP}"; \
    JAVA_PACKAGE_SHA256=$(curl -sSL "${JAVA_CHECKSUM}" | grep -E "${JAVA_ASSET}" | grep -Eo '(sha256: )[^<]+' | cut -d: -f2 | xargs) && \
    echo "${JAVA_PACKAGE_SHA256}  ${JAVA_TMP}" | sha256sum -c; \
	mkdir -p "${JAVA_HOME}"; \
    tar --extract --file "${JAVA_TMP}" --directory "${JAVA_HOME}" --strip-components 1; \
	ln -s "$JAVA_HOME/bin/"* "/usr/bin/"; \
	ls -lp "${JAVA_JVM_PATH}"; \
# Clean up
	find ${JAVA_HOME} -maxdepth 1 -mindepth 1 | grep -v jre | xargs rm -rf && \
	apk del glibc-i18n; \
	apk del --no-network .java-deps; \
	if ls $JAVA_HOME/*src.zip; then echo "rm -rf $JAVA_HOME/*src.zip"; fi; \
	if test -f $JAVA_HOME/jre/bin/javaws; then echo "rm -rf $JAVA_HOME/jre/bin/javaws"; fi; \
	if test -f $JAVA_HOME/jre/bin/jjs; then echo "rm -rf $JAVA_HOME/jre/bin/jjs"; fi; \
	if test -f $JAVA_HOME/jre/bin/keytool; then echo "rm -rf $JAVA_HOME/jre/bin/keytool"; fi; \
	if test -f $JAVA_HOME/jre/bin/orbd; then echo "rm -rf $JAVA_HOME/jre/bin/orbd"; fi; \
	if test -f $JAVA_HOME/jre/bin/pack200; then echo "rm -rf $JAVA_HOME/jre/bin/pack200"; fi; \
	if test -f $JAVA_HOME/jre/bin/policytool; then echo "rm -rf $JAVA_HOME/jre/bin/policytool"; fi; \
	if test -f $JAVA_HOME/jre/bin/rmid; then echo "rm -rf $JAVA_HOME/jre/bin/rmid"; fi; \
	if test -f $JAVA_HOME/jre/bin/rmiregistry; then echo "rm -rf $JAVA_HOME/jre/bin/rmiregistry"; fi; \
	if test -f $JAVA_HOME/jre/bin/servertool; then echo "rm -rf $JAVA_HOME/jre/bin/servertool"; fi; \
	if test -f $JAVA_HOME/jre/bin/tnameserv; then echo "rm -rf $JAVA_HOME/jre/bin/tnameserv"; fi; \
	if test -f $JAVA_HOME/jre/bin/unpack200; then echo "rm -rf $JAVA_HOME/jre/bin/unpack200"; fi; \
	if ls $JAVA_HOME/jre/lib/*javafx*; then echo "rm -rf $JAVA_HOME/jre/lib/*javafx*"; fi; \
	if ls $JAVA_HOME/jre/lib/*jfx*; then echo "rm -rf $JAVA_HOME/jre/lib/*jfx*"; fi; \
	if test -f $JAVA_HOME/jre/lib/amd64/libdecora_sse.so; then echo "rm -rf $JAVA_HOME/jre/lib/amd64/libdecora_sse.so"; fi; \
	if test -f $JAVA_HOME/jre/lib/amd64/libfxplugins.so; then echo "rm -rf $JAVA_HOME/jre/lib/amd64/libfxplugins.so"; fi; \
	if test -f $JAVA_HOME/jre/lib/amd64/libglass.so; then echo "rm -rf $JAVA_HOME/jre/lib/amd64/libglass.so"; fi; \
	if test -f $JAVA_HOME/jre/lib/amd64/libgstreamer-lite.so; then echo "rm -rf $JAVA_HOME/jre/lib/amd64/libgstreamer-lite.so"; fi; \
	if ls $JAVA_HOME/jre/lib/amd64/libjavafx*.so; then echo "rm -rf $JAVA_HOME/jre/lib/amd64/libjavafx*.so"; fi; \
	if ls $JAVA_HOME/jre/lib/amd64/libjfx*.so; then echo "rm -rf $JAVA_HOME/jre/lib/amd64/libjfx*.so"; fi; \
	if ls $JAVA_HOME/jre/lib/amd64/libprism_*.so; then echo "rm -rf $JAVA_HOME/jre/lib/amd64/libprism_*.so"; fi; \
	if ls $JAVA_HOME/jre/lib/deploy*; then echo "rm -rf $JAVA_HOME/jre/lib/deploy*"; fi; \
	if test -f $JAVA_HOME/jre/lib/desktop; then echo "rm -rf $JAVA_HOME/jre/lib/desktop"; fi; \
	if test -f $JAVA_HOME/jre/lib/ext/jfxrt.jar; then echo "rm -rf $JAVA_HOME/jre/lib/ext/jfxrt.jar"; fi; \
	if test -f $JAVA_HOME/jre/lib/ext/nashorn.jar; then echo "rm -rf $JAVA_HOME/jre/lib/ext/nashorn.jar"; fi; \
	if test -f $JAVA_HOME/jre/lib/javaws.jar; then echo "rm -rf $JAVA_HOME/jre/lib/javaws.jar"; fi; \
	if test -f $JAVA_HOME/jre/lib/jfr; then echo "rm -rf $JAVA_HOME/jre/lib/jfr"; fi; \
	if test -f $JAVA_HOME/jre/lib/jfr.jar; then echo "rm -rf $JAVA_HOME/jre/lib/jfr.jar"; fi; \
	if test -f $JAVA_HOME/jre/lib/oblique-fonts; then echo "rm -rf $JAVA_HOME/jre/lib/oblique-fonts"; fi; \
	if test -f $JAVA_HOME/jre/lib/plugin.jar; then echo "rm -rf $JAVA_HOME/jre/lib/plugin.jar"; fi; \
	if test -f $JAVA_HOME/jre/plugin; then echo "rm -rf $JAVA_HOME/jre/plugin"; fi; \
	if ls $JAVA_HOME/lib/*javafx*; then echo "rm -rf $JAVA_HOME/lib/*javafx*"; fi; \
	if test -f $JAVA_HOME/lib/missioncontrol; then echo "rm -rf $JAVA_HOME/lib/missioncontrol"; fi; \
	if test -f $JAVA_HOME/lib/visualvm; then echo "rm -rf $JAVA_HOME/lib/visualvm"; fi; \
	rm -rf \
			/tmp/* /var/cache/apk/*; \
# Java smoke test
	which java; \
	# java -version; \
	which javac; \
	ls -lp "${JAVA_HOME}/lib"; \
	ls -lp "${JAVA_HOME}/jre/bin"; \
	ls -lp "${JAVA_HOME}/jre/lib"
	# javac -version

# https://docs.oracle.com/javase/10/tools/jshell.htm
# https://docs.oracle.com/javase/10/jshell/
# https://en.wikipedia.org/wiki/JShell
CMD ["jshell"]
