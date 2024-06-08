# Start from ubuntu 22.04
FROM ubuntu:22.04

# Update/upgrade the image and install curl
RUN apt update
RUN apt-get install -y curl

# Use /usr/src/app as our workdir. The following instructions will be executed in this location.
WORKDIR /usr/src/app

# Copy the script.sh
COPY script.sh .

# Alternatively, if we skipped chmod earlier, we can add execution permissions during the build.
RUN chmod +x script.sh

# When running docker run the command will be ./hello.sh
ENTRYPOINT ["./script.sh"]
