# BUILD
FROM node:14 AS build

# install client
WORKDIR /tmp/client
COPY . .
RUN npm ci
RUN npm run build

# DEPLOY
FROM nginx

# setup the environment
WORKDIR /opt/superdesk/client/dist

# build client
COPY --from=build /tmp/client/dist ./

RUN rm /etc/nginx/conf.d/default.conf
COPY ./docker/nginx.conf /etc/nginx/nginx.conf
COPY ./docker/superdesk_vhost.conf /etc/nginx/sites-enabled/superdesk.conf
COPY ./docker/start.sh /opt/superdesk/start.sh

ENTRYPOINT [ "/opt/superdesk/start.sh" ]
CMD ["nginx", "-g daemon off;"]
