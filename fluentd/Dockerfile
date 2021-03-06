FROM centos:centos7

MAINTAINER Anton Sherkhonov <antons@redhat.com>

ENV HOME=/opt/app-root/src \
  PATH=/opt/app-root/src/bin:/opt/app-root/bin:$PATH \
  RUBY_VERSION=2.0 \
  FLUENTD_VERSION=0.12.23 \
  GEM_HOME=/opt/app-root/src

LABEL io.k8s.description="Fluentd container for collecting of docker container logs" \
  io.k8s.display-name="Fluentd ${FLUENTD_VERSION}" \
  io.openshift.tags="logging,fluentd"

# activesupport version 5.x requires ruby 2.2
RUN rpmkeys --import file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7 && \
    yum install -y --setopt=tsflags=nodocs \
      gcc-c++ \
      ruby \
      ruby-devel \
      libcurl-devel \
      make \
      iproute \
      epel-release && \
    yum install -y zeromq3 && \
    yum clean all
RUN mkdir -p ${HOME} && \
    gem install --no-rdoc --no-ri \
      --conservative --minimal-deps \
      fluentd:${FLUENTD_VERSION} \
      'activesupport:<5' \
#      fluent-plugin-kubernetes_metadata_filter \
      fluent-plugin-elasticsearch \
#      fluent-plugin-flatten-hash \
#      fluent-plugin-systemd \
      systemd-journal \
      fluent-plugin-rewrite-tag-filter \
      fluent-plugin-fedmsg

ADD run.sh ${HOME}/

RUN mkdir -p /etc/fluent/configs.d/ && \
    ln -s /usr/lib64/libzmq.so.3 /usr/local/lib/libzmq.so

WORKDIR ${HOME}
USER 0
CMD ["sh", "run.sh"]
