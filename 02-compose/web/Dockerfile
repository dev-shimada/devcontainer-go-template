FROM node:23.1-bookworm-slim AS vscode

WORKDIR /workspace/message-web

RUN <<EOF
apt-get update
apt-get install -y git vim curl
EOF
COPY . .
# RUN <<EOF
# npm install --prefix ./message-web
# EOF

# CMD ["npm", "run", "dev", "--prefix", "./nextjs-blog"]
