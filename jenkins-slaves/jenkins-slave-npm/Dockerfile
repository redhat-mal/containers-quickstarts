#invoke npm in jenkinsfile: sh "scl enable rh-nodejs6 'npm run build'"
FROM quay.io/openshift/origin-jenkins-agent-base:4.3

ENV NODEJS_VERSION=12 \
    NPM_CONFIG_PREFIX=$HOME/.npm-global \
    PATH=$HOME/node_modules/.bin/:$HOME/.npm-global/bin/:$PATH 

ADD ubi.repo /etc/yum.repos.d/ubi.repo

#RUN curl --silent https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm --output google-chrome-stable_current_x86_64.rpm && \
RUN curl --silent --location https://rpm.nodesource.com/setup_${NODEJS_VERSION}.x | bash -

RUN INSTALL_PKGS="nodejs " && \
    DISABLE_REPOS=--disablerepo='rhel-*' && \
    yum $DISABLE_REPOS install -y yum-utils && \
    yum -y --setopt=tsflags=nodocs $DISABLE_REPOS install $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum -y clean all --enablerepo='*' && \
#    yum install -y --setopt=tsflags=nodocs \
#      $INSTALL_PKGS && \
#    yum -y localinstall \
#      google-chrome-stable_current_x86_64.rpm && \
#    rm google-chrome-stable_current_x86_64.rpm && \
#    rpm -V $INSTALL_PKGS google-chrome-stable && \
#    yum clean all -y && \
#    rm -rf /var/cache/yum && \
    npm install --unsafe-perm -g npm-audit-html npm-audit-ci-wrapper sonar-scanner || cat /home/jenkins/.npm/_logs/*-debug.log && \
    chown root:root /home/jenkins -R && \
    chmod 775 /home/jenkins/.config -R && \
    chmod 775 /home/jenkins/.npm -R

USER 1001
