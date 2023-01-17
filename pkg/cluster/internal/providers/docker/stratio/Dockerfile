FROM kindest/node:v1.24.6

# Tools versions
ENV CLUSTERCTL=v1.2.4
ENV CLUSTERAWSADM=v1.5.0
ENV CAPAVERSION=v1.5.1

# Init feature gates
ENV CLUSTER_TOPOLOGY="true"

RUN mkdir -p /root/.cluster-api/local-repository/cluster-api/${CLUSTERCTL} \
    && mkdir -p /root/.cluster-api/local-repository/bootstrap-kubeadm/${CLUSTERCTL} \
    && mkdir -p /root/.cluster-api/local-repository/control-plane-kubeadm/${CLUSTERCTL} \
    && mkdir -p /root/.cluster-api/local-repository/infrastructure-aws/${CAPAVERSION} \
    && touch /root/.cluster-api/local-repository/cluster-api/${CLUSTERCTL}/core-components.yaml \
    && touch /root/.cluster-api/local-repository/cluster-api/${CLUSTERCTL}/metadata.yaml \
    && touch /root/.cluster-api/local-repository/bootstrap-kubeadm/${CLUSTERCTL}/bootstrap-components.yaml \
    && touch /root/.cluster-api/local-repository/bootstrap-kubeadm/${CLUSTERCTL}/metadata.yaml \
    && touch /root/.cluster-api/local-repository/control-plane-kubeadm/${CLUSTERCTL}/control-plane-components.yaml \
    && touch /root/.cluster-api/local-repository/control-plane-kubeadm/${CLUSTERCTL}/metadata.yaml \
    && touch /root/.cluster-api/local-repository/infrastructure-aws/${CAPAVERSION}/infrastructure-components.yaml \
    && touch /root/.cluster-api/local-repository/infrastructure-aws/${CAPAVERSION}/metadata.yaml

# Download clusterctl (61.5M)
RUN curl -L https://github.com/kubernetes-sigs/cluster-api/releases/download/${CLUSTERCTL}/clusterctl-linux-amd64 -o /usr/local/bin/clusterctl \
    && chmod +x /usr/local/bin/clusterctl

# Download clusterawsadm (92.5M)
RUN curl -L https://github.com/kubernetes-sigs/cluster-api-provider-aws/releases/download/${CLUSTERAWSADM}/clusterawsadm-linux-amd64 -o /usr/local/bin/clusterawsadm \
    && chmod +x /usr/local/bin/clusterawsadm

#Download providers offline

RUN curl -L https://github.com/kubernetes-sigs/cluster-api/releases/download/${CLUSTERCTL}/core-components.yaml \
    -o /root/.cluster-api/local-repository/cluster-api/${CLUSTERCTL}/core-components.yaml

RUN curl -L https://github.com/kubernetes-sigs/cluster-api/releases/download/${CLUSTERCTL}/metadata.yaml \
    -o /root/.cluster-api/local-repository/cluster-api/${CLUSTERCTL}/metadata.yaml

RUN curl -L https://github.com/kubernetes-sigs/cluster-api/releases/download/${CLUSTERCTL}/bootstrap-components.yaml \
    -o /root/.cluster-api/local-repository/bootstrap-kubeadm/${CLUSTERCTL}/bootstrap-components.yaml 

RUN curl -L https://github.com/kubernetes-sigs/cluster-api/releases/download/${CLUSTERCTL}/control-plane-components.yaml \
    -o /root/.cluster-api/local-repository/control-plane-kubeadm/${CLUSTERCTL}/control-plane-components.yaml

RUN curl -L https://github.com/kubernetes-sigs/cluster-api-provider-aws/releases/download/${CAPAVERSION}/infrastructure-components.yaml \
    -o /root/.cluster-api/local-repository/infrastructure-aws/${CAPAVERSION}/infrastructure-components.yaml

RUN curl -L https://github.com/kubernetes-sigs/cluster-api-provider-aws/releases/download/${CAPAVERSION}/metadata.yaml \
    -o /root/.cluster-api/local-repository/infrastructure-aws/${CAPAVERSION}/metadata.yaml

RUN cp /root/.cluster-api/local-repository/cluster-api/${CLUSTERCTL}/metadata.yaml  /root/.cluster-api/local-repository/bootstrap-kubeadm/${CLUSTERCTL}/metadata.yaml \
    && cp /root/.cluster-api/local-repository/cluster-api/${CLUSTERCTL}/metadata.yaml  /root/.cluster-api/local-repository/control-plane-kubeadm/${CLUSTERCTL}/metadata.yaml

RUN echo '\
providers:\n\
- name: cluster-api\n\
  url: /root/.cluster-api/local-repository/cluster-api/${CLUSTERCTL}/core-components.yaml\n\
  type: CoreProvider\n\
- name: kubeadm \n\
  url: /root/.cluster-api/local-repository/bootstrap-kubeadm/${CLUSTERCTL}/bootstrap-components.yaml\n\
  type: BootstrapProvider\n\  
- name: kubeadm\n\
  url: /root/.cluster-api/local-repository/control-plane-kubeadm/${CLUSTERCTL}/control-plane-components.yaml\n\
  type: ControlPlaneProvider\n\  
- name: aws\n\
  url: /root/.cluster-api/local-repository/infrastructure-aws/${CAPAVERSION}/infrastructure-components.yaml\n\
  type: InfrastructureProvider\n' >> /root/.cluster-api/clusterctl.yaml
