CyVerse Ansible k3s
===================

This role will apply a yaml to k8s and make an ingress using the given ports.

Requirements
------------

This role assumes you have k8s or some variant such as k3s set up, and it was tested with k3s.
It also assumes you have any services you want to use set up already.

Role Variables
--------------

The following table lists optional ansible variables along with the default values if not defined.

Variable Name | Default value if not defined | Description
------------- | ---------------------- | -----------
K8S_RESOURCE  | None | a required variable that is the .yaml file you want applied or an http address to one, can also be a list of .ymal files.
IS_HTTP       | true | indicates that you are using a http link to a yaml file. set to false to use a string of a full file.
IS_BASE64     | false | indicates that you are using a base64 version of a file string. set to false to use a normal string.
K3S_PORTS     | None | a required variable that is a list of "port-service-name:port_numbers" such as ["port-servicea:80","port-serviceb:8080"] which will be used in the ingress

Example Playbook
----------------

This is a sample playbook:
````
- hosts: k3s_cluster
  become: true
  roles:
    - ansible-k8s-apply
  vars:
    IS_HTTP: false
    K8S_RESOURCE: "apiVersion: apps/v1\nkind: Deployment\nmetadata:\n  name: nginx-deployment\n  labels:\n    app: nginx\nspec:\n  replicas: 3\n  selector:\n    matchLabels:\n      app: nginx\n  template:\n    metadata:\n      labels:\n        app: nginx\n    spec:\n      containers:\n      - name: nginx\n        image: nginx:1.14.2\n        ports:\n        - containerPort: 80"
    K3S_PORTS: ["port-servicea:80","port-serviceb:8080"]
````

Author Information
------------------
Ryan Schneider (rschneider@cyverse.org)
