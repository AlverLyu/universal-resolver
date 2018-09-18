# Dockerfile

FROM debian:stretch
MAINTAINER Markus Sabadello <markus@danubetech.com>

USER root

RUN apt-get -y update

RUN apt-get install -y --no-install-recommends openjdk-8-jdk maven

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
ENV PATH $JAVA_HOME/bin:$PATH

# build dependencies

RUN apt-get install -y --no-install-recommends git

RUN cd /opt/ && git clone https://github.com/decentralized-identity/did-common-java.git
RUN cd /opt/did-common-java/ && mvn clean install -DskipTests

RUN cd /opt/ && git clone https://github.com/decentralized-identity/universal-resolver-java.git
RUN cd /opt/universal-resolver-java && mvn clean install -N -DskipTests
RUN cd /opt/universal-resolver-java/uni-resolver-core && mvn install -N -DskipTests
RUN cd /opt/universal-resolver-java/driver && mvn install -N -DskipTests

# build driver-did-erc725

ADD . /opt/driver-did-erc725
RUN cd /opt/driver-did-erc725 && mvn install package -N -DskipTests

# clean up

RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# variables

ENV uniresolver_driver_did_erc725_ethereumConnection jsonrpc
ENV uniresolver_driver_did_erc725_rpcUrlMainnet https://mainnet.infura.io/9W2fvWQMP6cJCMH3ESqP
ENV uniresolver_driver_did_erc725_rpcUrlRopsten https://ropsten.infura.io/9W2fvWQMP6cJCMH3ESqP
ENV uniresolver_driver_did_erc725_rpcUrlRinkeby https://rinkeby.infura.io/9W2fvWQMP6cJCMH3ESqP
ENV uniresolver_driver_did_erc725_rpcUrlKovan https://kovan.infura.io/9W2fvWQMP6cJCMH3ESqP
ENV uniresolver_driver_did_erc725_etherscanApiMainnet http://api.etherscan.io/api
ENV uniresolver_driver_did_erc725_etherscanApiRopsten http://api-ropsten.etherscan.io/api
ENV uniresolver_driver_did_erc725_etherscanApiRinkeby http://api-rinkeby.etherscan.io/api
ENV uniresolver_driver_did_erc725_etherscanApiKovan http://api-kovan.etherscan.io/api

# done

EXPOSE 8080

RUN chmod a+rx /opt/driver-did-erc725/docker/run-driver-did-erc725.sh
CMD "/opt/driver-did-erc725/docker/run-driver-did-erc725.sh"