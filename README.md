## MTproxy
Config directiry
```bash
mkdir ~/mtproxy && cd ~/mtproxy
```
Gen fake tls with spec SNI (microsoft.com, ya.ru, yandex.com, google.com)
```bash
docker run --rm nineseconds/mtg:2 generate-secret --hex microsoft.com
```
Copy hex output like "ee473ce5d4958eb5f968c87680a23854..."

Create new Toml 
```
nano config.toml
```
```Toml
secret = "ee473ce5d4958eb5f968c87680a23854......."
bind-to = "0.0.0.0:4443"
```
save  Ctrl+O → Enter → Ctrl+X
Run docker
```bash
docker run -d \
  --name mtproxy \
  --restart=unless-stopped \
  -v $(pwd)/config.toml:/config.toml \
  -p 4443:4443 \
  nineseconds/mtg:2
  ```
  Generate link
  ```
  docker exec mtproxy /mtg access /config.toml
  ```
  Output like
  ```
  url: tg://proxy?server=IP....&port=4443&secret=ddSECRET....
  ```
Config Firewall 
```
sudo ufw allow 4443/tcp
sudo ufw reload
sudo ufw enable
```






