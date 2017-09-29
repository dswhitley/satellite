promote role
============

This is a role which will promote a content view to a specific lifecycle
environment.  This can get tricky, but the initial iteration will only promote
a single version of a content view to a lifecycle environment.

There is only a little logic currently.  The role is expected to be passed both
a content view name and a lifecycle environment name.  This may not make sense
as you would typically promote a specific version to a specific lifecycle.  So
we take the lifecycle argument, find the prior configured lifecycle and promote
*THAT* version to the lifecycle environment that is passed.

**THEREFORE** it may make the most sense to promote the higher environments
first (Production, Staging, QA, Dev, etc).  Otherwise, if you promote to the
lowest environment first...it is likely that Dev will be promoted to the same
content view version as "Library" and then QA will be promoted to it's prior
environment (Dev) which is now at the same content view version as Library.  
This will essentially put all Lifecycle Environments at the same Content View
version as Library and it is unlikely that that is the desired effect.  ***so be
careful in the order of promotion!***

The *promote* task is not run asynchronously.  That means that each promotion
will wait for the previous to finish.  This can add some time, but is overall
a better approach.

Requirements
------------

This role currently uses a lot of `hammer` commands (along with some command
  line parsing of the output).  So `hammer` must be configured, which is
  essentially ensuring that the `foreman.yml` file is populated correctly
  (possibly in `/etc/hammer/cli.modules.d/`).

Role Variables
--------------

The ***promote*** role relies on the Fully Qualified Domain name be contained in a
`sat_fqdn` variable.  In my example, that is in a top level `global_vars.yml`
file which is "included" in the main playbook, for me, titled `updates.yml`

Additionally, the role needs to be passed these two variables:
* **content_view**: this is the name of a content view
* **lifecycle_env**: this is the name of a lifecycle environment to promote the
(prior) content view version to

Example Playbook
----------------

An example of how to use the role:

    - hosts: servers
      roles:
        - publish # without variables, this will publish everything
        - { role: promote, content_view: 'CCV RHEL 7', lifecycle_env: 'Dev' }

TO-DO
-----

Ideally this role should allow for two variables which will modify its behavior:
* ***CONTENTVIEWVER***:  instead of just promoting the content view version of
the prior content view version, allow this parameter to specify the version

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
