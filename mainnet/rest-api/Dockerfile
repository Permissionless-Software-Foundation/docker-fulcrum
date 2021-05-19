# Create a REST API for the Fulcrum Electrumx server.

FROM christroutner/ct-base-ubuntu
MAINTAINER Chris Troutner <chris.troutner@gmail.com>

RUN apt-get update -y

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

EXPOSE 3000
#VOLUME /data

CMD ["bash", "start-rest-api.sh"]

# Debugging
#COPY dummyapp.js dummyapp.js
#CMD ["node", "dummyapp.js"]
