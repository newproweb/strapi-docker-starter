

# dev | prod
ARG RUN_MODE=dev

# === Dev Mode ===

FROM node:18 as dev


WORKDIR /app

ADD ./ /app

RUN npm install

ENTRYPOINT [ "npm", "run", "develop" ]



# === Dev Mode ===

FROM node:18 as prod

WORKDIR /app

ADD ./ /app

RUN npm install


ENTRYPOINT [ "npm", "start"]

# Pick the container from selected run mode
FROM {RUN_MODE}
            