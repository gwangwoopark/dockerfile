FROM ubuntu:20.04

ARG USER_NAME="gwangwoo"
ARG GROUP_NAME="gwangwoo"
ARG USER_PASSWORD="welcome!"
ARG HOME="/home/${USER_NAME}"
ARG DEBIAN_FRONTEND=noninteractive

ARG GIT_USER="Gwangwoo Park"
ARG GIT_EMAIL="gwangwoo.park@gmail.com"

# ENV USER_NAME ${USER_NAME}
# ENV USER_PASSWORD ${USER_PASSWORD}
# ENV GROUP_NAME ${GROUP_NAME}

# install the tooks i wish to use
RUN apt-get update && \
  apt-get install -y sudo \
  apt-utils \
  software-properties-common \
  locales

RUN apt-get install -y curl \
  wget

RUN apt-get install -y git \
  git-lfs

RUN apt-get install -y zsh \
  neovim \
  tmux \
  tig \
  jq \
  bat

RUN apt-get install -y python3-pip
  
RUN add-apt-repository ppa:aacebedo/fasd && \
  apt-get update && \
  apt-get install -y fasd


RUN add-apt-repository ppa:dawidd0811/neofetch && \
  apt-get update && \
  apt-get install -y neofetch

# set up locale
RUN locale-gen en_US.UTF-8

# add a user (--disabled-password: the user won't be able to use the account until the password is set)
RUN groupadd ${GROUP_NAME} \
  && adduser --quiet --disabled-password --shell /bin/zsh --home ${HOME} --gecos "User" ${USER_NAME} \
  # update the password
  && echo "${USER_NAME}:${USER_PASSWORD}" | chpasswd && usermod -g ${GROUP_NAME} -aG sudo ${USER_NAME}

RUN curl -fsSL https://get.docker.com/ | sh
RUN usermod -aG docker ${USER_NAME}

COPY .p10k.zsh /tmp_config/
COPY .zshrc /tmp_config/
RUN chown ${USER_NAME}:${GROUP_NAME} /tmp_config/.*

# the user we're applying this too (otherwise it most likely install for root)
USER ${USER_NAME}

RUN sh -c "$(curl https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)" --unattended
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${HOME}/.oh-my-zsh/custom/plugins/zsh-autosuggestions
RUN git clone https://github.com/zsh-users/zsh-completions ${HOME}/.oh-my-zsh/custom/plugins/zsh-completions
RUN git clone https://github.com/zsh-users/zsh-history-substring-search ${HOME}/.oh-my-zsh/custom/plugins/zsh-history-substring-search
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting ${HOME}/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting
RUN git clone https://github.com/romkatv/powerlevel10k ${HOME}/.oh-my-zsh/custom/themes/powerlevel10k
RUN curl -sLf https://spacevim.org/install.sh | bash
RUN git clone --depth 1 https://github.com/junegunn/fzf.git ${HOME}/.fzf
RUN ${HOME}/.fzf/install
RUN git clone https://github.com/gpakosz/.tmux.git ${HOME}/.tmux
RUN ls -al ${HOME}/.tmux/
RUN ln -s -f ${HOME}/.tmux/.tmux.conf ${HOME}/.tmux.conf
RUN cp ${HOME}/.tmux/.tmux.conf.local ${HOME}

RUN cp -rp /tmp_config/. ${HOME}

WORKDIR ${HOME}

RUN git config --global user.name ${GIT_USER} \
  && git config --global user.email ${GIT_EMAIL} \
  && git config --global core.editor "vim"

ENTRYPOINT [ "/bin/zsh" ]
