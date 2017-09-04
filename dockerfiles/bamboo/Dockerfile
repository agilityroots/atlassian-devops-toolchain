FROM cptactionhank/atlassian-bamboo:6.0.3
USER root

# install executables (maven, ansible etc) in bamboo server to perform builds.
# apt-transport-https is needed, but cannot be installed via apt-get due to some config nuances of base image.
RUN wget http://ftp.us.debian.org/debian/pool/main/a/apt/apt-transport-https_1.0.9.8.4_amd64.deb \
  && dpkg -i apt-transport-https_1.0.9.8.4_amd64.deb
RUN apt-get update -qqy && apt-get install -qy maven
