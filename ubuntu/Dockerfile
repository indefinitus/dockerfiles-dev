FROM ubuntu:latest


# Set the locale
RUN apt-get update && apt-get install -y \
    locales \
    && rm -rf /var/lib/apt/lists/*
RUN sed -i '/en_US.UTF-8/s/^# //g' /etc/locale.gen && \
    locale-gen
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8


# install common packages
RUN apt-get update && apt-get install -y \
    curl \
    git \
    zsh \
    wget \
    sudo \
    vim \
    iputils-ping \
    iproute2 \
    net-tools \
    dnsutils \
    && rm -rf /var/lib/apt/lists/*

# timezone setup
ENV TZ=Asia/Singapore
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
    tzdata \
    && rm -rf /var/lib/apt/lists/*

# add user
# https://stackoverflow.com/questions/27701930/how-to-add-users-to-docker-container
RUN useradd -rm -d /home/admin -s /bin/zsh -g root -G sudo -u 1000 -p "$(openssl passwd -1 admin)" admin
USER admin
WORKDIR /home/admin

# install omz
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# change default zsh theme
RUN sed -i 's/^ZSH_THEME=.*/ZSH_THEME="crcandy"/' ~/.zshrc
RUN sed -i 's/^plugins=.*/plugins=(git zsh-syntax-highlighting zsh-autosuggestions zsh-completions brew command-time sudo)/' ~/.zshrc

# zsh plugins
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
RUN git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions
RUN git clone https://github.com/popstas/zsh-command-time.git ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/command-time

RUN echo "autoload -U compinit && compinit" >> ~/.zshrc
RUN echo "ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE=\"fg=7\"" >> ~/.zshrc
RUN echo "ZSH_COMMAND_TIME_MIN_SECONDS=0" >> ~/.zshrc
RUN echo 'ZSH_COMMAND_TIME_MSG="Execution time: %s"' >> ~/.zshrc

CMD ["zsh"]
