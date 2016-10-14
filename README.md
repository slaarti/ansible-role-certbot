Certbot
=======

This role manages installing and setting up [Let's Encrypt][letsencrypt]'s
[Certbot][certbot] for registering SSL certificates. Installation will be
done via `apt` package, from the Backports repository for the current
Debian distribution.

[letsencrypt]: https://letsencrypt.org/
[certbot]: https://certbot.eff.org/

It assumes that you'll be using [Nginx](http://nginx.org/) as your web
server, and will have the `cron` job that renews your certificates restart
Nginx accordingly. First, however, it will have Certbot use its standalone
web server to get the initial certificates for all of your domains, then
rewrite the renewal configuration files to use webroot authentication
instead. You should, therefore, use this role before whatever roles you
use to manage Nginx.

Role Variables
--------------

    certbot_default_release: "{{ansible_distribution_release}}-backports"

Default release for installing packages. Pre-configured to specify the
backports repository for your Debian installation.

    certbot_config_directory: "/etc/letsencrypt"

Certbot configuration directory, where things like your account
information live.

    certbot_account_list: []

A list of hashes containing configuration information on Let's Encrypt
authentication servers and credentials. It is assumed that you've already
installed `certbot` somewhere else and used `certbot register` to create
your account and create the files whose information goes into this
variable. Because this includes private key information, you should use
Ansible Vault to secure any file you put this variable in. Each account in
this list has the following elements, each of which is required:

*   `server`: The URL of the certbot server you want to register against
    to get your certificates. You will need to specify either the Let's
    Encrypt production or staging servers if you're going for real certs,
    or the Boulder CA server of your choice if you're doing some private
    testing or other work.

*   `account_id`: The ID for your account on this server. It's an MD5 hash
    hex digest of some information derived from your account's public key.
    It is used in the directory path leading to where the keys and other
    information for this account are stored.

*   `meta`, `privatekey`, and `regr`: The single-line JSON contents of the
    meta, private key, and registration files for the account,
    respectively. You should probably use `|` or `>` block literal
    notation for these elements, to save on having to otherwise escape any
    of the quotations in the files.

Dependencies
------------

This role depends on [`jnv.debian-backports`][backports] to set up the
Debian Backports repository. While not an actual dependency, I recommend
the [`geerlingguy.nginx` role][geerlingguy] to manage installing Nginx.

[geerlingguy]: https://github.com/geerlingguy/ansible-role-nginx

Example Playbook
----------------

    - hosts: servers
      roles:
         - slaarti.certbot

License
-------

Unlicense; see the LICENSE file.

Author Information
------------------

Chris Pinard <slaarti@gmail.com>
