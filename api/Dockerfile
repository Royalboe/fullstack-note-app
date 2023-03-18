FROM node:lts

WORKDIR /usr/app

COPY ./package.json .
RUN npm install --production --force

COPY . .

CMD [ "npm", "run", "start" ]