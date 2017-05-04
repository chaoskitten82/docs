# Set Up a Local Development Environment with MAMP

#### Purpose: Outline the process for setting up a local development environment on Mac for the purpose of committing code to a remote repository for deployment to a staging or production environment.

- - -

**Quick Steps:**

1. Create Site Root
2. Configure MAMP
3. Spoof Hosts File
4. Install NodeJS and Build Tools


### 1. Create Site Root or Clone a Remote Repository

On your Mac, create a directory in `/Users/yourname/Sites/mysite/public_html` where `mysite` is your client... OR clone a remote repository to `/Users/yourname/Sites/`.

- - -

### 2. Configure MAMP (Local Apache and MySQL Servers)

Download and install MAMP: https://www.mamp.info/en/downloads/

**Preferences:**

* Webserver > Apache, Set Document Root (`/User/yourname/Sites`)
* Ports > Apache 80, MySQL 3306

**MAMP VirtualHost Conf:**

In `/Applications/MAMP/conf/apache/httpd.conf` uncomment the following around line 574:

_Modify:_

```bash
# Virtual hosts
Include /Applications/MAMP/conf/apache/extra/httpd-vhosts.conf
```

In `/Applications/MAMP/conf/apache/extra/httpd-vhosts.conf,` add a VirtualHost for your site:

_Modify:_

```bash
# NameVirtualHost *:80
NameVirtualHost *
```

_Add:_

```bash
<VirtualHost *>
    ServerName mysite.dev
    DocumentRoot "/Users/yourname/Sites/mysite/public_html"
</VirtualHost>
```

- - -

### 3. Spoof Local Hosts File

In `/etc/hosts` add the following for your site:

_Add:_

```bash
127.0.0.1 mysite.dev
```

After saving your changes, you are now ready to start MAMP.

**Note:** Modifying or adding new virtualhost configurations to MAMP will require MAMP Apache to be restarted.

- - -

### 4. Install NodeJS and Build Tools

Download and install Node.js: https://nodejs.org/en/

Install Gulp/Grunt and whatever other packages you want globally for your development environment.

**Note:**
If you receive errors when pulling down packages with connection to Github, it might be related to a local network firewall preventing use of the `git://` protocol. Run the following to update your local git config to the `https://` protocol.

```bash
git config --global url."https://".insteadOf git://
```
