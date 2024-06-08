# Start from alpine image that is smaller but no fancy tools
FROM ubuntu:22.04

# Update the image and install curl
RUN apt-get update && apt-get upgrade -y && apt-get install -y curl


# Use /usr/src/app as our workdir. The following instructions will be executed in this location.
WORKDIR /usr/src/app

# Copy the script.sh
COPY script.sh .

# Alternatively, if we skipped chmod earlier, we can add execution permissions during the build.
RUN chmod +x script.sh

# When running docker run the command will be ./hello.sh
CMD ./script.sh
