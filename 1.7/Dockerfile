FROM golang:1.7-alpine
MAINTAINER AusNimbus <support@ausnimbus.com.au>

EXPOSE 8080

ENV STI_SCRIPTS_PATH=/usr/libexec/s2i \
    HOME=/opt/app-root/src

LABEL io.openshift.s2i.scripts-url=image:///usr/libexec/s2i

RUN apk add --update bash python git && rm -rf /var/cache/apk/* && \
    mkdir -p ${HOME}/.pki/nssdb && \
    chown -R 1001:0 ${HOME}/.pki && \
    adduser -S -u 1001 -G root -h ${HOME} -s /sbin/nologin \
      -g "Default Application User" default && \
    chown -R 1001:0 /opt/app-root

# Copy executable utilities.
COPY bin/ /usr/bin/

ENV GOPATH=$HOME/go \
    PATH=$GOPATH/bin:/usr/local/go/bin:$PATH

LABEL summary="Platform for building and running Golang ${GOLANG_VERSION} applications" \
    io.k8s.description="Platform for building and running Golang ${GOLANG_VERSION} applications" \
    io.k8s.display-name="Golang ${GOLANG_VERSION}" \
    io.openshift.expose-services="8080:http" \
    io.openshift.tags="builder,go,golang,go${GOLANG_VERSION}"

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# Each language image can have 'contrib' a directory with extra files needed to
# run and build the applications.
COPY ./contrib/ /opt/app-root

# Directory with the sources is set as the working directory so all STI scripts
# can execute relative to this path.
WORKDIR ${HOME}

# Drop the root user and make the content of /opt/app-root owned by user 1001
RUN chown -R 1001:0 /opt/app-root && chmod -R ug+rwx /opt/app-root
USER 1001

CMD $STI_SCRIPTS_PATH/usage
ENTRYPOINT ["container-entrypoint"]
