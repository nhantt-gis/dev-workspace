FROM oraclelinux:9

# Set environment variables
ENV TERM=xterm-256color
ENV COLORTERM=truecolor
ENV LANG=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8
ENV SHELL=/bin/zsh
ENV USER=devuser
ENV HOME=/home/devuser

# Install system packages
RUN dnf update -y && \
    dnf install -y \
        zsh \
        file \
        git \
        curl \
        wget \
        vim \
        nano \
        tmux \
        unzip \
        tar \
        gzip \
        tree \
        jq \
        which \
        procps-ng \
        net-tools \
        sudo \
    dnf clean all

# Install neovim
RUN curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.tar.gz && \
    rm -rf /opt/nvim && \
    tar -C /opt -xzf nvim-linux-x86_64.tar.gz

# Install Zinit
RUN bash -c "$(curl --fail --show-error --silent --location https://raw.githubusercontent.com/zdharma-continuum/zinit/HEAD/scripts/install.sh)"

# Create non-root user
RUN useradd -m -s /bin/zsh $USER && \
    usermod -aG wheel $USER && \
    echo "$USER ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Switch to non-root user
USER $USER
WORKDIR $HOME

# Create necessary directories
RUN mkdir -p $HOME/.local/share $HOME/.config

# Copy ZSH configuration files
COPY --chown=$USER:$USER docker/zsh/.zshrc $HOME/.zshrc
COPY --chown=$USER:$USER docker/zsh/.zsh_aliases $HOME/.zsh_aliases
COPY --chown=$USER:$USER docker/zsh/.zinit-config $HOME/.zinit-config

# Create workspace directory as root first
USER root
RUN mkdir -p /workspace && chown $USER:$USER /workspace

# Fix cache permissions
RUN mkdir -p /home/$USER/.cache && chown -R $USER:$USER /home/$USER/.cache

# Switch back to non-root user
USER $USER

# Set up history persistence
RUN mkdir -p $HOME/.zsh_history_data

WORKDIR /workspace

# Default command
CMD ["/bin/zsh"]
