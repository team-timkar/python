# Dockerfile for Python 3.8 with pipenv
FROM python:3.14.0a7-bookworm

ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# Install necessary tools
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    git \
    sudo \
    zsh \
    postgresql-client \
    && rm -rf /var/lib/apt/lists/*

# Upgrade pip
RUN pip install --upgrade pip && \
    rm -rf /root/.cache/pip/*

# Create a non-root user
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Create the workspace directory and set permissions
WORKDIR /workspaces
RUN mkdir -p /workspaces && chown -R $USERNAME:$USERNAME /workspaces

# Set up home directory for vscode user
RUN mkdir -p /home/$USERNAME/.vscode-server \
    && chown -R $USERNAME:$USERNAME /home/$USERNAME

    # Set zsh as the default shell
RUN chsh -s /usr/bin/zsh vscode

# Switch to non-root user for subsequent commands
USER $USERNAME