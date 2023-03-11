FROM node:12-stretch

USER node

# set the working directory in the container
# if the dir doesnt exist, it will create it for you, but on root...
WORKDIR /home/node/code

COPY --chown=node:node index.js index.js
# source > destination

CMD ["node", "index.js"]
