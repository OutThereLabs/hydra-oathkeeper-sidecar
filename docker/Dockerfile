ARG OATHKEEPER_VERSION=v0.14.2_oryOS.10
ARG ALPINE_VERSION=3.7

FROM oryd/oathkeeper:$OATHKEEPER_VERSION

FROM alpine:$ALPINE_VERSION

ENV BUILD_DEPS="gettext" \
    RUNTIME_DEPS="libintl curl"

RUN set -x && \
    apk add --no-cache --update $RUNTIME_DEPS && \
    apk add --virtual build_deps $BUILD_DEPS &&  \
    cp /usr/bin/envsubst /usr/local/bin/envsubst && \
    apk del build_deps

COPY --from=0 /usr/bin/oathkeeper /usr/bin/oathkeeper
COPY bin/oathkeeper-sidecar /usr/bin/oathkeeper-sidecar 
COPY rules /etc/oathkeeper-sidecar/rules

VOLUME [ "/etc/oathkeeper-sidecar/" ]

# For Basic Operations
ENV DATABASE_URL=memory \
    CREDENTIALS_ISSUER_ID_TOKEN_HS256_SECRET=arandomsecretarandomsecretarando \
    OATHKEEPER_API_URL=http://127.0.0.1:4456

# For Default Rules
ENV OATHKEEPER_RESOURCE_PREFIX=resources:oathkeeper: \
    HYDRA_URL=http://127.0.0.1:4444 \
    HYDRA_ADMIN_URL=http://127.0.0.1:4445 \
    OATHKEEPER_PROXY_URL=http://hydra:4455

# For Authenticators
ENV AUTHENTICATOR_OAUTH2_INTROSPECTION_URL="http://127.0.0.1:4444/oauth2/introspect" \
    AUTHENTICATOR_OAUTH2_CLIENT_CREDENTIALS_TOKEN_URL="http://127.0.0.1:4444/oauth2/token" \
    CREDENTIALS_ISSUER_ID_TOKEN_HYDRA_ADMIN_URL="http://127.0.0.1:4445"

# For Authorizors
ENV AUTHORIZER_KETO_URL=http://127.0.0.1:4466

EXPOSE 4455

CMD ["/usr/bin/oathkeeper-sidecar"]
