FROM node:22-slim

WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install

COPY . .

ENV PLAYWRIGHT_BROWSERS_PATH=/usr/local/share/playwright

# Install Playwright dependencies
RUN npx playwright install chromium --with-deps

RUN npm run build

EXPOSE 3003

COPY --from=ghcr.io/tarampampam/microcheck:1.3.0 /bin/httpcheck /bin/httpcheck
HEALTHCHECK --start-period=30s --start-interval=2s --interval=1m --timeout=10s \
  CMD ["/bin/httpcheck", "http://localhost:3003/health"]

CMD [ "npm", "start" ]
