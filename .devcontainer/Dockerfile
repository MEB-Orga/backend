FROM mcr.microsoft.com/devcontainers/python:3.14

RUN apt-get update && apt-get install -y --no-install-recommends \
    sshpass \
    fastfetch \
    curl \
    meld \
    && rm -rf /var/lib/apt/lists/*

# SSH Setup
RUN mkdir -p /home/vscode/.ssh && chown -R vscode:vscode /home/vscode/.ssh

# Create Virtual Environment
ENV VIRTUAL_ENV=/opt/venv
RUN python3 -m venv $VIRTUAL_ENV

# Global PATH extension so non-interactive & interactive shells use venv
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

# Auto-activate for interactive bash shells (for both root and vscode user)
RUN echo "source /opt/venv/bin/activate" >> /etc/bash.bashrc && \
    echo "source /opt/venv/bin/activate" >> /home/vscode/.bashrc

# Docker CLI + Compose Plugin
RUN curl -fsSL https://download.docker.com/linux/debian/gpg \
      | gpg --dearmor -o /usr/share/keyrings/docker.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker.gpg] \
       https://download.docker.com/linux/debian bookworm stable" \
       > /etc/apt/sources.list.d/docker.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
       docker-ce-cli \
       docker-compose-plugin \
    && rm -rf /var/lib/apt/lists/*

# Install Python packages
# RUN pip install --no-cache-dir pylint