# Rails and Vue Developement-Environment

## Setup

## tree
```
.
├── README.md
├── api
│   ├── Dockerfile
│   ├── Gemfile
│   └── Gemfile.lock
├── docker-compose.yml
└── front
    └── Dockerfile
```

### api/Dockerfile
```
FROM ruby:2.7.1-alpine

ARG WORKDIR

ENV RUNTIME_PACKAGES="linux-headers libxml2-dev make gcc libc-dev nodejs tzdata postgresql-dev postgresql git" \
  DEV_PACKAGES="build-base curl-dev" \
  HOME=/${WORKDIR} \
  LANG=C.UTF-8 \
  TZ=Asia/Tokyo

RUN echo ${HOME}

WORKDIR ${HOME}

COPY Gemfile* ./

RUN apk update && \
  apk upgrade && \
  apk add --no-cache ${RUNTIME_PACKAGES} && \
  apk add --virtual build-dependencies --no-cache ${DEV_PACKAGES} && \
  bundle install -j4 && \
  apk del build-dependencies

COPY . .
```

### front/Dockerfile
```
FROM node:14.19.0-alpine

ARG WORKDIR

ENV HOME=/${WORKDIR} \
  LANG=C.UTF-8 \
  TZ=Asia/Tokyo \
  HOST=0.0.0.0

RUN yarn global add @vue/cli
WORKDIR ${HOME}

EXPOSE 8080
```

### env file
```sh
WORKDIR=app-name
POSTGRES_PASSWORD=db-password
CONTAINER_PORT=3001
API_PORT=3001
FRONT_PORT=8081
```

### docker
```sh
dc build
dc run api rails new . --force --database=postgresql --skip-bundle --webpacker --api
docker-compose build api
docker-compose run front vue create .
docker-compose up -d
```

### api/config/database.yml
```yml
default: &default
  adapter: postgresql
  encoding: unicode
  host: db            # 追加
  username: postgres  # 追加
  password: <%= ENV["POSTGRES_PASSWORD"] %>  # 追加
```
