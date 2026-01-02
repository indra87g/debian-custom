FROM --platform=$TARGETOS/$TARGETARCH debian:bookworm

LABEL org.opencontainers.image.source="https://github.com/indra87g/debian-custom"
LABEL org.opencontainers.image.licenses=MIT

ENV DEBIAN_FRONTEND=noninteractive
ENV USER=container
ENV HOME=/home/container

# Install base dependencies
RUN apt update  && apt install -y --no-install-recommends \
    ca-certificates \
    curl \
    bash \
    git \
    build-essential \
	gcc \
    sqlite3 \
	iproute2 \
    libsqlite3-dev \
    unzip \
 && apt clean \
 && rm -rf /var/lib/apt/lists/*

# Install Bun
RUN curl -fsSL https://bun.sh/install | bash \
 && export BUN_INSTALL="${BUN_INSTALL:-/root/.bun}" \
 && cp -r "$BUN_INSTALL" /usr/local/.bun \
 && ln -sf /usr/local/.bun/bin/bun /usr/local/bin/bun \
 && chmod -R a+rX /usr/local/.bun \
 && /usr/local/.bun/bin/bun --version

# Create non-root user
RUN useradd -m -d /home/container -s /bin/bash container

USER container
WORKDIR /home/container

ENV PATH="/usr/local/.bun/bin:/home/container/.bun/bin:$PATH"

# Copy entrypoint
COPY --chown=container:container ./entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT [ "/bin/bash", "/entrypoint.sh" ]
