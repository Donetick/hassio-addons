ARG BUILD_FROM
FROM $BUILD_FROM

LABEL \
    org.opencontainers.image.title="Home Assistant Add-on: Donetick" \
    org.opencontainers.image.description="Simplify Tasks & Chores, Together." \
    org.opencontainers.image.source="https://github.com/donetick/hassio-addons" \
    org.opencontainers.image.licenses="GNU AFFERO GENERAL PUBLIC LICENSE"

WORKDIR /app

RUN apk --no-cache add gcc musl-dev git sqlite-dev curl jq
RUN mkdir -p /app/core

# Determine the architecture and download the appropriate binary
# TODO: am sure there is a better way to do this but this works for now
RUN arch=$(uname -m) && \
    case "$arch" in \
        x86_64) binary="donetick_Linux_x86_64.tar.gz" ;; \
        aarch64) binary="donetick_Linux_arm64.tar.gz" ;; \
        armv7l) binary="donetick_Linux_armv7.tar.gz" ;; \
        armv6l) binary="donetick_Linux_armv6.tar.gz" ;; \
        *) echo "Unsupported architecture: $arch" && exit 1 ;; \
    esac && \
    latest_release=$(curl --silent "https://api.github.com/repos/donetick/donetick/releases/latest" | jq -r .tag_name) && \
    curl -fL "https://github.com/donetick/donetick/releases/download/${latest_release}/${binary}" | tar -xz -C /app/core

# Copy entrypoint script
COPY entrypoint.sh /app/entrypoint.sh
RUN chmod +x /app/entrypoint.sh


# Expose necessary ports
EXPOSE 2021

# Set the entrypoint script
CMD ["/app/entrypoint.sh"]