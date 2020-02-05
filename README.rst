
users
*****

.. image:: https://gitlab.com/constrict0r/users/badges/master/pipeline.svg
   :alt: pipeline

.. image:: https://travis-ci.com/constrict0r/users.svg
   :alt: travis

.. image:: https://readthedocs.org/projects/users/badge
   :alt: readthedocs

Ansible role to create users.

.. image:: https://gitlab.com/constrict0r/img/raw/master/users/users.png
   :alt: users

Full documentation on `Readthedocs <https://users.readthedocs.io>`_.

Source code on:

`Github <https://github.com/constrict0r/users>`_.

`Gitlab <https://gitlab.com/constrict0r/users>`_.

`Part of: <https://gitlab.com/explore/projects?tag=doombots>`_

.. image:: https://gitlab.com/constrict0r/img/raw/master/users/doombots.png
   :alt: doombots

**Ingredients**

.. image:: https://gitlab.com/constrict0r/img/raw/master/users/ingredients.png
   :alt: ingredients


Contents
********

* `Description <#Description>`_
* `Usage <#Usage>`_
* `Variables <#Variables>`_
   * `users <#users>`_
   * `password <#password>`_
   * `configuration <#configuration>`_
* `YAML <#YAML>`_
* `Attributes <#Attributes>`_
   * `item_name <#item-name>`_
   * `item_pass <#item-pass>`_
   * `item_groups <#item-groups>`_
   * `item_expand <#item-expand>`_
   * `item_path <#item-path>`_
* `Requirements <#Requirements>`_
* `Compatibility <#Compatibility>`_
* `License <#License>`_
* `Links <#Links>`_
* `UML <#UML>`_
   * `Deployment <#deployment>`_
   * `Main <#main>`_
* `Author <#Author>`_

Description
***********

Ansible role to create users.

This role performs the following actions:

* Ensure the requirements are installed.

* Ensure the current user can obtain administrative (root)
   permissions.

* If the **users** variable is defined, create all users listed on
   it.

* If the **configuration** variable is defined, create all users
   listed on it.

* If the **password** variable is defined, set this password for all
   created users.

* If an user has defined an **item_pass** attribute, it will be
   setted as the password for the user.

* If an user has defined an **item_groups** attribute, it will be
   added to the groups listed on it.

If an user has a **item_pass** or **item_groups** attributes defined,
then it must have a non-empty **item_name** attribute defined too.


Usage
*****

* To install and execute:

..

   ::

      ansible-galaxy install constrict0r.users
      ansible localhost -m include_role -a name=constrict0r.users -K

* Passing variables:

..

   ::

      ansible localhost -m include_role -a name=constrict0r.users -K \
          -e "{users: [mary, jhon]}"

* To include the role on a playbook:

..

   ::

      - hosts: servers
        roles:
            - {role: constrict0r.users}

* To include the role as dependency on another role:

..

   ::

      dependencies:
        - role: constrict0r.users
          users: [mary, jhon]

* To use the role from tasks:

..

   ::

      - name: Execute role task.
        import_role:
          name: constrict0r.users
        vars:
          users: [mary, jhon]

* To run tests:

..

   ::

      cd users
      chmod +x testme.sh
      ./testme.sh

   On some tests you may need to use *sudo* to succeed.


Variables
*********

The following variables are supported:


users
=====

List of users to be created. Each non-empty username listed on users
will be created.

This list can be modified by passing an *users* array when including
the role on a playbook or via *–extra-vars* from a terminal.

This variable is empty by default.

::

   # Including from terminal.
   ansible localhost -m include_role -a name=constrict0r.users -K -e \
       "{users: [mary, jhon]}"

   # Including on a playbook.
   - hosts: servers
     roles:
       - role: constrict0r.users
         users:
           - mary
           - jhon

   # To a playbook from terminal.
   ansible-playbook -i tests/inventory tests/test-playbook.yml -K -e \
       "{users: [mary, jhon]}"


password
========

If an user do not specifies the *password* attribute, this password
will be setted for that user.

This password will only be setted for new users and do not affects
existent users.

This variable defaults to 1234.

::

   # Including from terminal.
   ansible localhost -m include_role -a name=constrict0r.users -K -e \
       "{password: 4321}"

   # Including on a playbook.
   - hosts: servers
     roles:
       - role: constrict0r.users
         password: 4321

   # To a playbook from terminal.
   ansible-playbook -i tests/inventory tests/test-playbook.yml -K -e \
       "password=4321"


configuration
=============

Absolute file path or URL to a *.yml* file that contains all or some
of the variables supported by this role.

It is recommended to use a *.yml* or *.yaml* extension for the
**configuration** file.

This variable is empty by default.

::

   # Using file path.
   ansible localhost -m include_role -a name=constrict0r.users -K -e \
       "configuration=/home/username/my-config.yml"

   # Using URL.
   ansible localhost -m include_role -a name=constrict0r.users -K -e \
       "configuration=https://my-url/my-config.yml"

To see how to write  a configuration file see the *YAML* file format
section.


YAML
****

When passing configuration files to this role as parameters, it’s
recommended to add a *.yml* or *.yaml* extension to the each file.

It is also recommended to add three dashes at the top of each file:

::

   ---

You can include in the file the variables required for your tasks:

::

   ---
   users:
     - [mary, jhon]

If you want this role to load list of items from files and URLs you
can set the **expand** variable to *true*:

::

   ---
   users: /home/username/my-config.yml

   expand: true

If the expand variable is *false*, any file path or URL found will be
treated like plain text.


Attributes
**********

On the item level you can use attributes to configure how this role
handles the items data.

The attributes supported by this role are:


item_name
=========

Name of the item to load or create.

::

   ---
   users:
     - item_name: my-item-name


item_pass
=========

Password for the item to load or create.

::

   ---
   users:
     - item_pass: my-item-pass


item_groups
===========

List of groups to add users into.

::

   ---
   users:
     - item_name: my-username
       item_groups: [disk, sudo]


item_expand
===========

Boolean value indicating if treat this item as a file path or URL or
just treat it as plain text.

::

   ---
   users:
     - item_expand: true
       item_path: /home/username/my-config.yml


item_path
=========

Absolute file path or URL to a *.yml* file.

::

   ---
   users:
     - item_path: /home/username/my-config.yml

This attribute also works with URLs.


Requirements
************

* `Ansible <https://www.ansible.com>`_ >= 2.8.

* `Jinja2 <https://palletsprojects.com/p/jinja/>`_.

* `Pip <https://pypi.org/project/pip/>`_.

* `Python <https://www.python.org/>`_.

* `PyYAML <https://pyyaml.org/>`_.

* `Requests <https://2.python-requests.org/en/master/>`_.

If you want to run the tests, you will also need:

* `Docker <https://www.docker.com/>`_.

* `Molecule <https://molecule.readthedocs.io/>`_.

* `Setuptools <https://pypi.org/project/setuptools/>`_.


Compatibility
*************

* `Debian Buster <https://wiki.debian.org/DebianBuster>`_.

* `Debian Raspbian <https://raspbian.org/>`_.

* `Debian Stretch <https://wiki.debian.org/DebianStretch>`_.

* `Ubuntu Xenial <http://releases.ubuntu.com/16.04/>`_.


License
*******

MIT. See the LICENSE file for more details.


Links
*****

* `Github <https://github.com/constrict0r/users>`_.

* `Gitlab <https://gitlab.com/constrict0r/users>`_.

* `Gitlab CI <https://gitlab.com/constrict0r/users/pipelines>`_.

* `Readthedocs <https://users.readthedocs.io>`_.

* `Travis CI <https://travis-ci.com/constrict0r/users>`_.


UML
***


Deployment
==========

The full project structure is shown below:

.. image:: https://gitlab.com/constrict0r/img/raw/master/users/deployment.png
   :alt: deployment


Main
====

The project data flow is shown below:

.. image:: https://gitlab.com/constrict0r/img/raw/master/users/main.png
   :alt: main


Author
******

.. image:: https://gitlab.com/constrict0r/img/raw/master/users/author.png
   :alt: author

The Travelling Vaudeville Villain.

Enjoy!!!

.. image:: https://gitlab.com/constrict0r/img/raw/master/users/enjoy.png
   :alt: enjoy

