# yatcm

Yet another tilde caddy manager. Runs with [bun](https://bun.sh).

## Configuration

### Caddy configuration over HTTP
This isn't reccommended if you're using a tilde as anyone can access it then.

Set your .env to this:

```bash
CADDY_ADMIN_PATH="http://localhost:2019/load"
# Or whatever port you chose for Caddy admin
```
### Caddy configuration over Unix Socket
This is better as you can control access to the socket.

```bash
# Leave CADDY_ADMIN_PATH alone.
CADDY_ADMIN_PATH="http://localhost/load"
CADDY_SOCKET_PATH="unix//run/caddy/caddy-admin.sock"
# Specify where you put your socket at.
```

Please protect your socket by only allowing admins/root to access it.


## Setup
1. Clone yactm to a safe location only root/admins can access.
   ```bash
   git clone https://github.com/aboutdavid/yactm.git
   ```
2. Modify your sudoers file to allow the execution of yactm as sudo.
   ```bash
   ALL ALL=(ALL) NOPASSWD: /var/run/yatcm/index.js
   ```
3. Add a link to yactm in your `/usr/bin`.
   ```bash
   #!/bin/bash
   sudo bun /var/run/yatcm/index.js
   ```
4. (optional) If you provide your users any subdomains, create a file called `.whitelist` and put a newline seperated list of domains, such as:
   ```bash
   *.freedomains.com
   *.freedomains.org
   ```
   Important: Put an astrisk where the subdomain goes!
5. Install the node modules.
   ```bash
   bun install
   ```
6. Modify the schema.prisma and create the database structure.
   ```
   npx prisma db push
   ```
7. You're done!

## Commands
```
Usage: yatcm [options] [command]

Yet another tilde caddy manager

Options:
  -V, --version           output the version number
  -h, --help              display help for command

Commands:
  list                    lists all domains you have configured in caddy
  add [options] <domain>  adds a domain to caddy (Use --proxy to proxy to another socket or URL)
  rm <domain>             removes a domain from caddy
  help [command]          display help for command
```