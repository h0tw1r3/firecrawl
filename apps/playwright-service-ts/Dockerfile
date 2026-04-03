FROM node:18-slim

WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install

COPY . .

ENV PLAYWRIGHT_BROWSERS_PATH=/usr/local/share/playwright

# Install Playwright dependencies
RUN npx playwright install chromium --with-deps

RUN npm run build

ARG PORT
ENV PORT=${PORT}

EXPOSE ${PORT}

COPY --from=ghcr.io/tarampampam/microcheck:1.3.0 /bin/httpcheck /bin/httpcheck
HEALTHCHECK --start-period=30s --start-interval=2s --interval=1m --timeout=10s \
  CMD ["/bin/httpcheck", "http://localhost:${PORT}/health"]

CMD [ "npm", "start" ]
