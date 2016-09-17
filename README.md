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

TBD.

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
