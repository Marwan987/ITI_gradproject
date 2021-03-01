Role Name
=========

A brief description of the role goes here.

Requirements
------------

-There's a file payload.json under files directory . In order to create a secret , you will have to insert your data there.

Role Variables
-------------e
 vault_url
 vault_token
 secret_engine_path
 secret_type
 secret_url

Example Payload.json
--------------------
{
  "data": {
    "foo2": "bar",
    "sszip2": "zap"
  }
}



Example Playbook
----------------

- name: Example on different usages of sonarqube role
  hosts: 127.0.0.1
  connection: local

  tasks:
   - name: Create a secret engine path
     import_role:
        name: vault_secret_management #Directory name containing this role
        tasks_from: create-secret-engine.yaml
     vars:
        vault_url: 127.0.0.1:8200   # Required
        vaultToken: "{{ ansible_env.VAULT_TOKEN }}"  # Required
        secret_engine_path: sonaruser  # Required Unique
        secret_type: kv-v2  # Required


   - name: Create a secret under specified secret engine path
     import_role:
        name: vault_secret_management #Directory name containing this role
        tasks_from: create-secret.yaml
     vars:
        vault_url: 127.0.0.1:8200   # Required
        token: "{{ ansible_env.VAULT_TOKEN }}"  # Required
        secret_engine_path: sonaruser  # Required Unique
        secret_path: sonar@123   # Required

   - name: Check available secrets at latest version
     import_role:
        name: vault_secret_management #Directory name containing this role
        tasks_from: check-secrets.yaml
     vars:
        vault_url: 127.0.0.1:8200   # Required
        vault_token: "{{ ansible_env.VAULT_TOKEN }}"  # Required
        secret_engine_path: sonaruser  # Required Unique

   - name: Check available secrets for the selected version
     import_role:
        name: vault_secret_management #Directory name containing this role
        tasks_from: check-certain-secret-version.yml
     vars:
        vault_url: 127.0.0.1:8200   # Required
        vault_token: "{{ ansible_env.VAULT_TOKEN }}"  # Required
        secret_engine_path: sonaruser  # Required Unique
        version_number: 1

