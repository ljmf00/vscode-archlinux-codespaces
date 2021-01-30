FROM archlinux
LABEL maintainer="Luís Ferreira <luis at aurorafoss dot org>"

# Add multilib repo
RUN echo -e "[multilib]\nInclude = /etc/pacman.d/mirrorlist" >> /etc/pacman.conf

# Install essentials, dev essentials and git
RUN pacman -Syu base base-devel \
	git \
	lib32-gcc-libs lib32-glibc --noprogressbar --needed --noconfirm

# Add user, group wheel
RUN /usr/sbin/groupadd --system wheel && \
    /usr/sbin/useradd -m --groups wheel vscode && \
    /usr/sbin/sed -i -e "s/Defaults    requiretty.*/ #Defaults    requiretty/g" /etc/sudoers && \
    /usr/sbin/echo '%wheel ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

WORKDIR /home/vscode/

# Install yay
RUN cd /tmp && \
    curl https://aur.archlinux.org/cgit/aur.git/plain/PKGBUILD?h=yay -o PKGBUILD && \
    sudo -u user makepkg -sic --noprogressbar --noconfirm && \
    rm -rf PKGBUILD yay*

# Set correct locale
RUN echo "LC_ALL=en_US.UTF-8" >> /etc/environment && \
    echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && \
    echo "LANG=en_US.UTF-8" > /etc/locale.conf

RUN locale-gen en_US.UTF-8
ENV LC_CTYPE 'en_US.UTF-8'

# Remove cache and update trusted certs
RUN rm -rf /var/cache/pacman/pkg/* && \
    rm -rf /var/lib/pacman/sync/* && \
    rm -rf /tmp/* && \
    trust extract-compat

CMD [ "sleep", "infinity" ]
