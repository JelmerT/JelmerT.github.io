Letsencrypt howto on osx:
-------------------------

use this to renew:
- exec letsencrypt tool (fix for missing path to libs):
sudo DYLD_LIBRARY_PATH=`brew --prefix`/lib ~/.local/share/letsencrypt/bin/letsencrypt certonly -a manual -d jelmertiete.com --debug

- update "letsencrypt-setup" with new codes when asked
- push change to gitlab and wait till deployed
- push enter in letsencrypt tool
- Now we just need to upload the certificate and the key to GitLab. Go to Settings -> Pages inside your gitlab project, remove the old CNAME and add a new one with the same domain, but now you'll also upload the TLS certificate. Paste the contents of /etc/letsencrypt/live/jelmertiete.com/fullchain.pem (you'll need sudo to read the file) to the "Certificate (PEM)" field and /etc/letsencrypt/live/jelmertiete.com/privkey.pem (also needs sudo) to the "Key (PEM)" field.