# cloudflare-docker-proxy

![deploy](https://github.com/ciiiii/cloudflare-docker-proxy/actions/workflows/deploy.yaml/badge.svg)

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/ciiiii/cloudflare-docker-proxy)

> If you're looking for proxy for helm, maybe you can try [cloudflare-helm-proxy](https://github.com/ciiiii/cloudflare-helm-proxy).

## Deploy

1. fork this project
2. modify the link of the above button to your fork url
3. click the button, you will be redirected to the deploy page

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/ciiiii/cloudflare-docker-proxy)

## Config tutorial

1. use cloudflare worker host: only support proxy one registry
   ```javascript
   const routes = {
     "${workername}.${username}.workers.dev/": "https://registry-1.docker.io",
   };
   ```
2. use custom domain: support proxy multiple registries route by host. Have to host your domain DNS on cloudflare.

  ```bash
  # replace libcuda.so to your domain in both src/index.js and wrangler.toml
  sed -i 's/libcuda.so/${your_domain}/g' src/index.js
  sed -i 's/libcuda.so/${your_domain}/g' wrangler.toml
  # install install dependencies
  yarn
  # deploy
  npx wrangler deploy --env production --minify src/index.js
  ```

## Usage

```bash
# busybox:stable ==> docker.libcuda.so/busybox:stable
docker pull docker.libcuda.so/busybox:stable
# or
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-EOF
{
    "registry-mirrors": [
        "https://docker.libcuda.so",
    ]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
docker pull busybox:stable
```
