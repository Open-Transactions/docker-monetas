FROM dockerfile/ubuntu

MAINTAINER Filip Gospodinov "filip@monetas.net"
# use bash for docker RUN commands; we want associative arrays
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# use custom GOPATH such that it does not conlfict with GOPATHs that
# live inside the host's $HOME which may be bind mounted into docker
ENV GOPATH /opt/go
ENV GOROOT /opt/go_dist/go
ENV PATH $GOROOT/bin:$GOPATH/bin:$PATH
ADD https://storage.googleapis.com/golang/go1.4.2.linux-amd64.tar.gz $HOME/
RUN mkdir /opt/go_dist $GOPATH && cd /opt/go_dist && tar xf $HOME/go1.4.2.linux-amd64.tar.gz
RUN declare -A deps=( \
    ["github.com/agl/ed25519"]="d2b94fd789ea21d12fac1a4443dd3a3f79cda72c" \
    ["github.com/conformal/btcutil/base58"]="40ba1daf690afbd356066e36c47ba0e4d071ddb0" \
    ["github.com/stretchr/testify"]="ae37dedcaae6fce497569786a8694aa3daa97abf"  \
    ["golang.org/x/crypto/ripemd160"]="632d287f9f3f54b09809eebbd3cacbcb00b9f2fc" \
    ) && \
    for dep in "${!deps[@]}"; do \
        go get "$dep"; \
        cd "$GOPATH/src/$dep"; \
        git checkout "${deps[$dep]}"; \
    done

CMD ["bash"]
