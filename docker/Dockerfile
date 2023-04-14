FROM node:alpine AS build-share
WORKDIR /app
RUN npm i -g pnpm

ARG PORT=3000
EXPOSE ${PORT}

FROM build-share AS dev
COPY ./package.json .
COPY ./pnpm-lock.yaml .
RUN pnpm install

COPY ./nest-cli.json .
COPY ./tsconfig*.json .
COPY ./src ./src
RUN pnpm run build

CMD [ "pnpm", "run", "start:dev" ]

FROM build-share AS install-production
COPY ./package.json .
COPY ./pnpm-lock.yaml .
RUN pnpm install -P

FROM alpine:latest AS production
WORKDIR /app
RUN apk add --no-cache nodejs
COPY --from=dev /app/dist /app/dist
COPY --from=install-production /app/node_modules /app/node_modules

CMD [ "node", "dist/main" ]
