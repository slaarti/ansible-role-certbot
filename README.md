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
Nginx accordingly.

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
authentication servers and credentials. Each account in this list has the
following elements, each of which is required:

*   `server`: The URL of the certbot server you want to register against
    to get your certificates. You will need to specify either the Let's
    Encrypt production or staging servers if you're going for real certs,
    or the Boulder CA server of your choice if you're doing some private
    testing or other work.

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
