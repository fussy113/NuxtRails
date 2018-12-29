FROM node:8.12-alpine as builder

ENV NODE_ENV=production

WORKDIR /var/www

# node_modulesの軽量化
RUN  apk add --no-cache curl git && cd /tmp && \
    curl -#L https://github.com/tj/node-prune/releases/download/v1.0.1/node-prune_1.0.1_linux_amd64.tar.gz | tar -xvzf- && \
    mv -v node-prune /usr/local/bin && rm -rvf * && \
    echo "yarn cache clean && node-prune" > /usr/local/bin/node-clean && chmod +x /usr/local/bin/node-clean

ADD ./package.json ./

RUN yarn --frozen-lockfile --non-interactive && node-clean

ADD ./ ./

RUN yarn build

FROM node:8.12-alpine

WORKDIR /var/www
ENV HOST=0.0.0.0
ENV PORT=3000

ADD package.json ./
ADD nuxt.config.js ./

# COPY --from=builder /var/www/server ./server/
COPY --from=builder /var/www/node_modules ./node_modules/
COPY --from=builder /var/www/.nuxt ./.nuxt/
COPY --from=builder /var/www/static ./static/

EXPOSE 3000
CMD ["yarn", "start"]
