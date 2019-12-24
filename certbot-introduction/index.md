# Certbot or Letsencrypt Introduction

2 Types of plugins:

- Authenticators
- Installers

### Authetenticators
Used with `certonly` command
Validates that you own the domain and then it places the cert in `/etc/letsencrypt/`.
It does not changes the Server (Web, FTP, Mail, etc)

### Installers
Used with the `install` command
They can modify the server configuration

### Both
Some plugins can do authetnticaiton and installing.
Used with the `run` command which is the default



