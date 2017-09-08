publish role
============

This is a role which will publish Content Views on a Satellite.  It will first
publish any "standalone" content views since the composite views will be
comprised of them.  It will then update the versions of each component of each
composite view, and then publish the composite content views.

Both the *standalone* and *composite* task files kick off the publish command in
an asynchronous manner.  They then use another `hammer` command to query the
tasks on the Satellite.  They are currently configured to wait 5 minutes between
attempts for 5 attempts.  

**NOTE** the `publish` command used could use some work because the results have
 been inconsistent.  I've attempted to allow the playbook continue in a few
 different scenarios I've seen.

Requirements
------------

This role currently uses a lot of `hammer` commands (along with some command
  line parsing of the output).  So `hammer` must be configured, which is
  essentially ensuring that the `foreman.yml` file is populated correctly
  (possibly in `/etc/hammer/cli.modules.d/`).

Role Variables
--------------

The `publish` role relies on the Fully Qualified Domain name be contained in a
`sat_fqdn` variable.  In my example, that is in a top level `global_vars.yml`
file which is "included" in the main playbook, for me, titled `updates.yml`

Additionally, the *composite* task gets passed a specific composite view to work
on.

Example Playbook
----------------

An example of how to use the role:

    - hosts: servers
      roles:
        - publish # without variables, this will publish everything

TO-DO
-----

Ideally this role should allow for two variables which will modify its behavior:
* ***CONTENTVIEW***:  a particular content view
* ***CHECKDATE***: only publish a content view if it is older than a
predetermined age

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
