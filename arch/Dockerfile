FROM archlinux:latest

RUN sed -i '/Color/s/^#//g' /etc/pacman.conf
RUN sed -i '/NoExtract.*man.*info/s/^/#/' /etc/pacman.conf

# install common packages
RUN pacman -Sy && pacman -S --noconfirm \
    curl \
    git \
    zsh \
    wget \
    sudo \
    vim \
    iproute2 \
    net-tools \
    man

# timezone setup
ENV TZ=Asia/Singapore

# add user
# https://stackoverflow.com/questions/27701930/how-to-add-users-to-docker-container
RUN useradd -rm -d /home/admin -s /bin/zsh -g wheel -u 1000 -p "$(openssl passwd -1 admin)" admin
RUN echo "admin ALL=(ALL:ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/dont-prompt-admin-for-password
USER admin
WORKDIR /home/admin

# install omz
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# change default zsh theme
RUN sed -i 's/^ZSH_THEME=.*/ZSH_THEME="crcandy"/' ~/.zshrc
RUN sed -i 's/^plugins=.*/plugins=(git zsh-syntax-highlighting zsh-autosuggestions zsh-completions brew command-time)/' ~/.zshrc

# zsh plugins
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
RUN git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/zsh-completions
RUN git clone https://github.com/popstas/zsh-command-time.git ${ZSH_CUSTOM:=~/.oh-my-zsh/custom}/plugins/command-time

RUN echo "alias vi=vim" >> ~/.zshrc
RUN echo "autoload -U compinit && compinit" >> ~/.zshrc
RUN echo "ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE=\"fg=7\"" >> ~/.zshrc
RUN echo "ZSH_COMMAND_TIME_MIN_SECONDS=0" >> ~/.zshrc
RUN echo 'ZSH_COMMAND_TIME_MSG="Execution time: %s"' >> ~/.zshrc

CMD ["zsh"]
