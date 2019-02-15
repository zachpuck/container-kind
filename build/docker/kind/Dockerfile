FROM docker:18.09.2-dind

ARG KUBECTL_VERSION=v1.13.0
ARG HELM_VERSION=2.12.1
ARG HELMFILE_VER=v0.54.1

RUN apk update && apk add --no-cache curl musl-dev git go python py-pip ca-certificates

# Configure docker
RUN mkdir -p /etc/docker/ && \
    echo '{"storage-driver": "vfs"}' > /etc/docker/daemon.json

# Configure go
ENV GOROOT=/usr/lib/go
ENV GOPATH=/go
ENV PATH=/go/bin:$PATH

RUN mkdir -p ${GOPATH}/src ${GOPATH}/bin

# Install kubectl
RUN curl -L https://storage.googleapis.com/kubernetes-release/release/${KUBECTL_VERSION}/bin/linux/amd64/kubectl -o /usr/local/bin/kubectl && \
    chmod +x /usr/local/bin/kubectl

# Install helm
ADD https://storage.googleapis.com/kubernetes-helm/helm-v${HELM_VERSION}-linux-amd64.tar.gz /tmp
RUN cd /tmp && \
    tar -xzf /tmp/helm-v${HELM_VERSION}-linux-amd64.tar.gz && \
    mv /tmp/linux-amd64/helm /usr/local/bin && \
    rm -rf /tmp/linux-amd64 /tmp/helm-v${HELM_VERSION}-linux-amd64.tar.gz && \
    helm init -c

# Install helm plugin tiller (for local tiller)
ENV HELM_HOST localhost:44134
RUN helm plugin install https://github.com/rimusz/helm-tiller

# install helmfile
RUN curl -sL --output /usr/local/bin/helmfile https://github.com/roboll/helmfile/releases/download/$HELMFILE_VER/helmfile_linux_amd64 && \
    chmod 755 /usr/local/bin/helmfile

# Install kind
RUN mkdir -p /go/src/sigs.k8s.io && \
    cd /go/src/sigs.k8s.io/ && \
    git clone https://github.com/kubernetes-sigs/kind.git && \
    go install sigs.k8s.io/kind && \
    rm -rf /go/src/sigs.k8s.io/

# Install AWS client
RUN pip install awscli --upgrade
RUN go get -u -v github.com/kubernetes-sigs/aws-iam-authenticator/cmd/aws-iam-authenticator && \
    rm -rf /go/src/
