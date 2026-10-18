FROM node:22-alpine

WORKDIR /usr/src/app

# First we only copy the json files to avoid the node_modules folder which causes error when we run the npm install
COPY package*.json ./

# Install dependencies and remove leftovers
RUN npm install \
    && rm -rf /usr/local/lib/node_modules/npm \
    && rm -rf /usr/local/bin/npm \
    && rm -rf /usr/local/bin/npx

# Copy all the content in the current repository
COPY . .

EXPOSE 8080

CMD [ "node", "app.js" ]

# For checking manually if the image work locally:
# 1) Open the cmd on sample-nodejs folder:
#   docker build -t my-sample-nodejs:latest -f deploy/Dockerfile .
#   docker run -d -p 8080:8080 --name running-sample-app my-sample-nodejs:latest
#
# 2) Open the web browser:
# http://localhost:8080/my-app (The main endpoint)
# http://localhost:8080/about (The about page)
# http://localhost:8080/metrics (The Prometheus metrics)


# To check vulnerabilities before we start we used:
# docker run --rm -v "$($PWD.Path):/app" -w /app node:22-alpine npm audit
# Which resulted in: 5 vulnerabilities (2 moderate, 2 high, 1 critical)

# To fix those vulnerabilities we used:
# docker run --rm -v "$($PWD.Path):/app" -w /app node:22-alpine npm audit fix
# Which created a new package-lock.json needed to create an image without those vulnerabilities
