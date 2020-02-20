FROM node:8-alpine

COPY installnode.sh /tmp
COPY installedgemicro.sh /tmp
#RUN chown=100:101 /tmp/installedgemicro.sh

# create user and group for microgateway
RUN apk add --no-cache sed grep && \
    addgroup -S apigee -g 101 && \
    adduser -s /bin/sh -u 100 -S -G apigee apigee -h /opt/apigee


ENV NODE_ENV production

#install node.js
RUN sh /tmp/installnode.sh && \
    rm -f /tmp/installnode.sh && \
    deluser --remove-home node

WORKDIR /opt/apigee
RUN mkdir /opt/apigee/.edgemicro && mkdir /opt/apigee/logs && mkdir /opt/apigee/plugins
VOLUME /opt/apigee/.edgemicro
VOLUME /opt/apigee/logs
VOLUME /opt/apigee/plugins

RUN whoami && \
    echo BuiltFolders

# copy tls files if needed
# COPY key.pem /opt/apigee/.edgemicro
# COPY cert.pem /opt/apigee/.edgemicro

# copy entrypoint
COPY entrypoint.sh /opt/apigee
RUN chown -R apigee:apigee /opt/apigee && \
    chown -R apigee:apigee /opt/apigee/.edgemicro && \
    chown apigee:apigee /opt/apigee/entrypoint.sh 
# initialize edgemicro
USER apigee
RUN whoami && \
    echo beforeRunningInstalledEdgemicro
    
RUN sh /tmp/installedgemicro.sh
    #rm -f /tmp/installedgemicro.sh

# Expose ports
EXPOSE 8000
EXPOSE 8443

ENTRYPOINT ["/opt/apigee/entrypoint.sh"]
