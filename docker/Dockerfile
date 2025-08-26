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
        sudo && \
    dnf clean all

# Install neovim
RUN curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.tar.gz && \
    rm -rf /opt/nvim && \
    tar -C /opt -xzf nvim-linux-x86_64.tar.gz && \
    rm nvim-linux-x86_64.tar.gz

# Install zinit (zsh plugin manager)
RUN bash -c "$(curl --fail --show-error --silent --location https://raw.githubusercontent.com/zdharma-continuum/zinit/HEAD/scripts/install.sh)"

# Install mise (runtime version manager)
RUN curl https://mise.run | sh

# Create non-root user
RUN useradd -m -s /bin/zsh $USER && \
    usermod -aG wheel $USER && \
    echo "$USER ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Create directories as root and set permissions
RUN mkdir -p /home/$USER/.local/share /home/$USER/.config /home/$USER/.local/bin && \
    chown -R $USER:$USER /home/$USER

# Switch to non-root user
USER $USER
WORKDIR $HOME

# Copy ZSH configuration files
COPY --chown=$USER:$USER docker/zsh/.zshrc $HOME/.zshrc
COPY --chown=$USER:$USER docker/zsh/.zsh_aliases $HOME/.zsh_aliases

# Copy other configuration files
COPY --chown=$USER:$USER docker/zinit/.zinit_config $HOME/.zinit_config
COPY --chown=$USER:$USER docker/zinit/starship.toml $HOME/.config/starship.toml
COPY --chown=$USER:$USER docker/mise/.mise.toml $HOME/.config/mise/config.toml

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
