FROM node:24-bookworm-slim

RUN groupadd --gid 1001 --system nodejs && \
  useradd --uid 1001 --system --gid nodejs nextjs

WORKDIR /app

COPY package*.json ./

RUN npm ci --omit=dev && \
  npm cache clean --force && \
  rm -rf /usr/local/lib/node_modules/npm /usr/local/bin/npm /usr/local/bin/npx /usr/local/bin/corepack

COPY --chown=nextjs:nodejs src/ ./src/

EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD node -e "require('http').get('http://localhost:3000/health', (res) => { process.exit(res.statusCode === 200 ? 0 : 1) })"

USER nextjs

CMD ["node", "src/simple-app.js"]