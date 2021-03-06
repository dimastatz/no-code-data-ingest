FROM alpine:3.5
LABEL maintainer ""
LABEL Description="Fluentd docker image" Vendor="Fluent Organization" Version="1.1"

ENV DUMB_INIT_VERSION=1.2.0

ENV SU_EXEC_VERSION=0.2

# install bash
RUN apk update && apk add bash

# Do not split this into multiple RUN!
# Docker creates a layer for every RUN-Statement
# therefore an 'apk delete' has no effect
RUN apk update \
 && apk add --no-cache \
        ca-certificates \
        ruby ruby-irb \
        su-exec==${SU_EXEC_VERSION}-r0 \
        dumb-init==${DUMB_INIT_VERSION}-r0 \
 && apk add --no-cache --virtual .build-deps \
        build-base \
        ruby-dev gnupg \
 && echo 'gem: --no-document' >> /etc/gemrc \
 && gem install oj -v 2.18.3 \
 && gem install json -v 2.1.0 \
 && gem install fluentd -v 0.14.25 \
 && gem install fluent-plugin-kafka --no-document \
 && gem install fluent-plugin-s3 -v 1.0.0 --no-document \
 && apk del .build-deps \
 && rm -rf /tmp/* /var/tmp/* /usr/lib/ruby/gems/*/cache/*.gem

# for log storage (maybe shared with host) and  configuration/plugins path (default: copied from .)
RUN mkdir -p /fluentd/log /fluentd/etc /fluentd/plugins

RUN addgroup -S fluent && adduser -S -g fluent fluent
RUN chown -R fluent /fluentd && chgrp -R fluent /fluentd

COPY fluent.conf /fluentd/etc/
COPY entrypoint.sh /bin/
RUN chmod 777 /bin/entrypoint.sh

ENV LD_PRELOAD=""
ENV DUMB_INIT_SETSID 0

EXPOSE 24224 5140

ENTRYPOINT ["/bin/entrypoint.sh"]
CMD ["fluentd"]
