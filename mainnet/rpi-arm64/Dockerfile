# Start with a pre-configured Ubuntu 18.04 image.
FROM ubuntu:18.04
MAINTAINER Chris Troutner <chris.troutner@gmail.com>

# Update the OS and install any OS packages needed.
RUN apt-get update
RUN apt-get install -y sudo git curl nano gnupg wget

# Create a safeuser
RUN useradd -ms /bin/bash safeuser
RUN adduser safeuser sudo

# Set password to 'abcd8765'. Change value below if you want a different password
RUN echo safeuser:abcd8765 | chpasswd

#Set the working directory to be the home directory
WORKDIR /home/safeuser

# Copy the Fulcrum binary and config file.
COPY bin/Fulcrum Fulcrum
COPY mainnet.conf mainnet.conf

# Copy SSL certificates
#COPY certs/fullchain.pem fullchain.pem
#COPY certs/privkey.pem privkey.pem
COPY certs/server.crt server.crt
COPY certs/server.key server.key

# Ensure all files are owned by the safeuser user.
RUN chown -R safeuser *

# Switch to user account.
USER safeuser
# Prep 'sudo' commands.
#RUN echo 'abcd8765' | sudo -S pwd

# Used to hold the Fulcrum database.
RUN mkdir blockchain

# Start Fulcrum server.
CMD ["./Fulcrum", "mainnet.conf"]

# Used for debugging.
#COPY bin/dummyapp.js dummyapp.js
#CMD ["node", "dummyapp.js"]
