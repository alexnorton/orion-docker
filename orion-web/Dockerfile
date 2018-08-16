FROM node:8 as builder

ARG sha
ARG mapbox_api_token

RUN wget -O orion-web.tar.gz https://github.com/LINKIWI/orion-web/archive/$sha.tar.gz
RUN mkdir /orion-web && tar -xvf orion-web.tar.gz -C /orion-web --strip-components=1

WORKDIR /orion-web

RUN npm install

# Patch Webpack config to work around https://github.com/webpack-contrib/css-loader/issues/447
RUN sed -i '94i\  node: { fs: "empty" },' webpack.config.js

ENV NODE_ENV production
ENV MAPBOX_API_TOKEN $mapbox_api_token

RUN npm run build

FROM nginx:1.14-alpine

ADD nginx.conf /etc/nginx/nginx.conf
COPY --from=builder /orion-web/dist /usr/share/nginx/html
