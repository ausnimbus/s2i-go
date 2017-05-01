#
# NOTE: THIS DOCKERFILE IS GENERATED VIA "./hack/run update"
#
# DO NOT EDIT IT DIRECTLY.
#
# <pre>

FROM golang:1.7-alpine
MAINTAINER AusNimbus <support@ausnimbus.com.au>

LABEL summary="Platform for running Golang 1.7 applications" \
      maintainer="AusNimbus <support@ausnimbus.com.au>" \
      io.k8s.description="Platform for running Golang 1.7 applications" \
      io.k8s.display-name="Golang 1.7"

ENV HOME=/opt/app-root/src

RUN apk --no-cache add bash rsync git && rm -rf /var/cache/apk/* && \
    mkdir -p ${HOME}/.pki/nssdb && \
    chown -R 1001:0 ${HOME}/.pki && \
    adduser -S -u 1001 -G root -h ${HOME} -s /sbin/nologin \
      -g "Default Application User" default && \
    chown -R 1001:0 /opt/app-root

# Copy common executable utilities.
COPY ./common/bin/ /usr/bin/

# </pre>
# <s2i>

EXPOSE 8080

ENV STI_SCRIPTS_PATH=/usr/libexec/s2i

LABEL io.openshift.expose-services="8080:http" \
    io.openshift.tags="builder,golang" \
    io.openshift.s2i.scripts-url=image:///usr/libexec/s2i

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# </s2i>
# <template>

ENV GOPATH=/opt/app-root \
    PATH=$GOPATH/bin:/usr/local/go/bin:$PATH

# </template>
# <post>

# Each container image can have 'contrib' a directory with extra files needed to
# run and build the application.
COPY ./contrib/ /opt/app-root

# Directory with the sources is set as the working directory so all STI scripts
# can execute relative to this path.
WORKDIR ${HOME}

# Drop the root user and make the content of /opt/app-root owned by user 1001
RUN chown -R 1001:0 /opt/app-root && chmod -R ug+rwx /opt/app-root
USER 1001

CMD /usr/bin/base-usage
ENTRYPOINT ["container-entrypoint"]

# </post>
