FROM quay.io/operator-framework/ansible-operator:v0.17.1

USER root

RUN yum -y update && yum clean all

RUN echo -ne "[centos-8-appstream]\nname = CentOS 8 (RPMs) - AppStream\nbaseurl = http://mirror.centos.org/centos-8/8/AppStream/x86_64/os/\nenabled = 1\ngpgcheck = 0" > /etc/yum.repos.d/centos.repo

RUN yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm \
 && yum -y install nss_wrapper \
 && yum clean all

RUN ln -s ${HOME}/.ansible /.ansible
COPY build/entrypoint /usr/local/bin/entrypoint
RUN chmod ugo+x /usr/local/bin/entrypoint

USER 1001

COPY build/requirements.yml ${HOME}/requirements.yml
RUN ansible-galaxy collection install -r ${HOME}/requirements.yml \
 && chmod -R ug+rwx ${HOME}/.ansible

COPY watches.yaml ${HOME}/watches.yaml

COPY roles/ ${HOME}/roles/