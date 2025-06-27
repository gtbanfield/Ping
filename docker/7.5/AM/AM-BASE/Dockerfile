#
# The contents of this file are subject to the terms of the Common Development and
# Distribution License (the License). You may not use this file except in compliance with the
# License.
#
# You can obtain a copy of the License at legal/CDDLv1.0.txt. See the License for the
# specific language governing permission and limitations under the License.
#
# When distributing Covered Software, include this CDDL Header Notice in each file and include
# the License file at legal/CDDLv1.0.txt. If applicable, add the following below the CDDL
# Header, with the fields enclosed by brackets [] replaced by your own identifying
# information: "Portions Copyright [year] [name of copyright owner]".
#
# Copyright 2021-2023 ForgeRock AS.
#

# Build arg required for customer builds
ARG docker_tag=latest

FROM ${docker.push.repo}/am-empty:${docker_tag}

USER 0
RUN apt-get update -y && \
    apt-get install -y git jq unzip
USER 11111

ARG VERSION="7.0.0-SNAPSHOT"

COPY --chown=forgerock:root build/base-config.tar "$FORGEROCK_HOME"/base-config.tar
COPY --chown=forgerock:root build/crypto-tool.jar "$FORGEROCK_HOME"/crypto-tool.jar
COPY --chown=forgerock:root homeVersion "$AM_HOME"/.homeVersion
COPY --chown=forgerock:root boot.json $AM_HOME/config
COPY --chown=forgerock:root docker-entrypoint.sh "$FORGEROCK_HOME"/
COPY --chown=forgerock:root scripts/import-pem-certs.sh "$FORGEROCK_HOME"/

WORKDIR "$FORGEROCK_HOME"
RUN mkdir base-config && \
    tar -xf base-config.tar -C base-config && \
    mv base-config/home/forgerock/openam/config/services $AM_HOME/config/services && \
    rm -rf base-config
COPY --chown=forgerock:root serverconfig.xml $AM_HOME/config/services/realm/root/iplanetamplatformservice/1.0/globalconfig/default/com-sun-identity-servers/

RUN mkdir -p $FORGEROCK_HOME/base/config && \
    cp -r $AM_HOME/config/services $FORGEROCK_HOME/base/config && \
    mkdir -p $AM_HOME/config/auth/ace/data

COPY --chown=forgerock:root log4j.properties "$AM_HOME"/config/auth/ace/data/
COPY --chown=forgerock:root rsa_api.properties "$AM_HOME"/config/auth/ace/data/

WORKDIR "$AM_HOME"
RUN git config --global user.email "ci@forgerock.com" && \
    git config --global user.name "ForgeRock" && \
    git init && \
    git add . && \
    git commit --quiet -m "Base configuration"

RUN rm "$CATALINA_HOME"/webapps/am/WEB-INF/lib/click-extras-*.jar
RUN rm "$CATALINA_HOME"/webapps/am/WEB-INF/lib/click-nodeps-*.jar
RUN rm "$CATALINA_HOME"/webapps/am/WEB-INF/lib/velocity-*.jar

ENV FBC_BASE_PATHS=""

WORKDIR /usr/local/tomcat

CMD "$FORGEROCK_HOME/docker-entrypoint.sh"
