FROM node:14.19.0-alpine

ARG WORKDIR

ENV HOME=/${WORKDIR} \
  LANG=C.UTF-8 \
  TZ=Asia/Tokyo \
  HOST=0.0.0.0

RUN yarn global add @vue/cli
WORKDIR ${HOME}

EXPOSE 8080