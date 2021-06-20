FROM node:14.14.0-alpine

USER node
 
RUN mkdir -p /home/node/app
WORKDIR /home/node/app
 
COPY --chown=node:node ./package.json ./
RUN npm install

# even though we are using volumes, keep this COPY command
# to act as a placeholder for when we focus on production
COPY --chown=node:node ./ ./
 
CMD ["npm", "run", "start"]