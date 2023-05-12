# Create a REST API for the Fulcrum Electrumx server.

# Start with a pre-configured Ubuntu 18.04 image.
FROM ubuntu:18.04
MAINTAINER Chris Troutner <chris.troutner@gmail.com>

# Update the OS and install any OS packages needed.
RUN apt-get update
RUN apt-get install -y sudo git curl nano gnupg wget

#Install Node and NPM
RUN curl -sL https://deb.nodesource.com/setup_16.x -o nodesource_setup.sh
RUN bash nodesource_setup.sh
RUN apt-get install -y nodejs build-essential

# Create a safeuser
RUN useradd -ms /bin/bash safeuser
RUN adduser safeuser sudo

# Set password to 'abcd8765'. Change value below if you want a different password
RUN echo safeuser:abcd8765 | chpasswd

#Set the working directory to be the home directory
WORKDIR /home/safeuser

# Switch to user account.
USER safeuser
# Prep 'sudo' commands.
#RUN echo 'abcd8765' | sudo -S pwd

RUN git clone https://github.com/Permissionless-Software-Foundation/fulcrum-api
WORKDIR /home/safeuser/fulcrum-api
RUN npm ci
COPY start-rest-api.sh start-rest-api.sh

#EXPOSE 3000
#VOLUME /data

CMD ["bash", "start-rest-api.sh"]

# Debugging
#COPY dummyapp.js dummyapp.js
#CMD ["node", "dummyapp.js"]
