FROM node:alpine
WORKDIR /app
RUN npm i -g pnpm

COPY package.json .
COPY pnpm-lock.yaml .
RUN pnpm install

COPY nest-cli.json .
COPY tsconfig*.json .

COPY ./src ./src

ARG PORT=3000
EXPOSE ${PORT}

CMD [ "pnpm", "run", "start:dev" ]
