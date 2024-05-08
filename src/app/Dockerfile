FROM node:16-alpine3.13

ARG IMAGE_CREATE_DATE
ARG IMAGE_VERSION
ARG IMAGE_SOURCE_REVISION

# Metadata as defined in OCI image spec annotations - https://github.com/opencontainers/image-spec/blob/master/annotations.md
LABEL org.opencontainers.image.title="Docker Demo!" \
      org.opencontainers.image.description="Provides a demo app to deploy to a Kubernetes cluster. It displays a message, the name of the pod and details of the node it is deployed to." \
      org.opencontainers.image.created=$IMAGE_CREATE_DATE \
      org.opencontainers.image.version=$IMAGE_VERSION \
      org.opencontainers.image.authors="Stephane Montri" \
      org.opencontainers.image.url="https://github.com/smontri/docker-demo" \
      org.opencontainers.image.documentation="https://github.com/smontri/docker-demo/blob/main/README.md" \
      org.opencontainers.image.vendor="Stephane Montri" \
      org.opencontainers.image.licenses="MIT" \
      org.opencontainers.image.source="https://github.com/smontri/docker-demo.git" \
      org.opencontainers.image.revision=$IMAGE_SOURCE_REVISION 

# Create app directory
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

# Install app dependencies
COPY package.json /usr/src/app/
RUN npm install

# Bundle app source
COPY . /usr/src/app

USER node
EXPOSE 8080
CMD [ "npm", "start" ]