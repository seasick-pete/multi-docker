FROM node:alpine as builder
USER node
RUN mkdir -p /home/node/app
WORKDIR /home/node/app
COPY --chown=node:node ./package.json ./
RUN npm install

# even though we are using volumes, keep this COPY command
# to act as a placeholder for when we focus on production
COPY --chown=node:node ./ ./
RUN npm run build

FROM nginx
EXPOSE 3000
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf
COPY --from=0 /home/node/app/build /usr/share/nginx/html