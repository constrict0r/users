Description
--------------------------------------------------------------

Ansible role to create users.

This role performs the following actions:

- Ensure the requirements are installed.

- Ensure the current user can obtain administrative (root) permissions.

- If the **users** variable is defined, create all users listed on it.

- If the **configuration** variable is defined, create all users listed on it.

- If the **password** variable is defined, set this password for all created
  users.

- If an user has defined an **item_pass** attribute, it will be setted as the
  password for the user.

- If an user has defined an **item_group** attribute, it will be added to the
  groups listed on it.

If an user has a **item_pass** or **item_group** attributes defined, then it
must have a non-empty **item_name** attribute defined too.