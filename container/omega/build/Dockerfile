FROM alpine AS downloader
WORKDIR /download
RUN     apk add --no-cache wget && \
        wget -q -O OmegaManager https://cdn.cftools.de/om/linux/OmegaManager && \
        wget -q -O manager.cfg https://cdn.cftools.de/om/manager.cfg

FROM ubuntu:20.04
RUN     apt-get update && \
        `# SteamCMD dependencies` \
        apt-get install -y ca-certificates locales lib32gcc-s1 && \
        `# Container dependencies` \
        apt-get install -y sed bash jq bsdmainutils && \
        `# OmegaManager dependencies` \
        apt-get install -y libc6 zlib1g && \
        `# DayZ Server dependencies` \
        apt-get install -y libcurl4 libc6 libcap2 libstdc++6 libgcc-s1 libnghttp2-14 libidn2-0 libssh-4 libpsl5 libssl1.1 libgssapi-krb5-2 libldap-2.4-2 libldap-2.4-2 libbrotli1 zlib1g libunistring2 libgnutls30 libhogweed5 libnettle7 libgmp10 libkrb5-3 libk5crypto3 libcomerr2 libkrb5support0 libsasl2-2 libgssapi3-heimdal libbrotli1 libp11-kit0 libtasn1-6 libkeyutils1 libheimntlm0-heimdal libkrb5-26-heimdal libasn1-8-heimdal libhcrypto4-heimdal libroken18-heimdal libffi7 libwind0-heimdal libheimbase1-heimdal libhx509-5-heimdal libsqlite3-0 libcrypt1 && \
        apt-get clean -y
WORKDIR /opt/cftools/omega-manager
RUN     mkdir -p \
                certs \
                changelists \
                conf \
                deploy \
                logs \
                servers \
                steamcmd \
                workshopdeploy \
                .internal && \
        useradd --home /opt/cftools/omega-manager/conf omegamanager && \
        sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
        locale-gen && \
        rm /etc/machine-id && \
        ln -s -T /opt/cftools/omega-manager/conf/manager.cfg            manager.cfg && \
        ln -s -T /opt/cftools/omega-manager/conf/machine-id             /etc/machine-id && \
        ln -s -T /opt/cftools/omega-manager/OmegaManager                /bin/OmegaManager && \
        ln -s -T /opt/cftools/omega-manager/Setup                       /bin/Setup
COPY --from=downloader /download/OmegaManager ./
COPY --from=downloader /download/manager.cfg ./.internal/.template_manager.cfg
COPY resources/Setup ./
RUN     chown -R omegamanager:omegamanager . && \
        chmod 755 ./OmegaManager && \
        chmod 755 /opt/cftools
USER omegamanager:omegamanager

ENTRYPOINT ["/bin/bash", "-c"]
CMD ["OmegaManager"]