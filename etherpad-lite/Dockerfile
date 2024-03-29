FROM node:latest
LABEL maintainer="Tom Broughton <docker@tombroughton.me>"


# git hash of the version to be built.
# If not given, build the latest development version.
ARG ETHERPAD_VERSION=develop

# Set the following to production to avoid installing devDeps
# this can be done with build args (and is mandatory to build ARM version)
ARG NODE_ENV=development

# add dependencies to system including cypto libraries
RUN apt-get update && \
    apt-get install -y curl \
    	     -yqq \
	     python-bcrypt \
	     mysql-client
    	    	      

# grab the ETHERPAD_VERSION tarball from github (no need to clone the whole
# repository)
RUN echo "Getting version: ${ETHERPAD_VERSION}" && \
	curl \
		--location \
		--fail \
		--silent \
		--show-error \
		--output /opt/etherpad-lite.tar.gz \
		https://github.com/ether/etherpad-lite/archive/"${ETHERPAD_VERSION}".tar.gz && \
	mkdir /opt/etherpad-lite && \
	tar xf /opt/etherpad-lite.tar.gz \
		--directory /opt/etherpad-lite \
		--strip-components=1 && \
	rm /opt/etherpad-lite.tar.gz

# install node dependencies for Etherpad
RUN /opt/etherpad-lite/bin/installDeps.sh

# Copy the custom configuration file, if present. The configuration file has to
# be manually put inside the same directory containing the Dockerfile (we cannot
# directly point to "../settings.json" for Docker's security restrictions).
#
# For the conditional COPY trick, see:
#   https://stackoverflow.com/questions/31528384/conditional-copy-add-in-dockerfile#46801962
COPY nop setting[s].json /opt/etherpad-lite/


EXPOSE 9001
WORKDIR /opt/etherpad-lite

# make sure we have the dependecy for our hashes
RUN npm i bcrypt -save
# install the hash auth checker
RUN npm i ep_hash_auth

CMD ["node", "node_modules/ep_etherpad-lite/node/server.js"]
