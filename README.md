# ansible-mongodb-3.6-initial-setup

Ansible playbook to configure mongodb for replication ready, and to configure datadog to monitor mongodb metrics. This playbooks only work for ubuntu machines. This playbook configures several things:
- set hostname of mongodb instance.
- mount and format ebs volume for mongodb to /var/lib/mongodb
- set datadog key for datadog agent
- configure mongodb log stream to cloudwatch logs
- configure root user for mongodb with temporary password, you must to rotate it later.
- configure mongodb service so it is ready to be joined into a replication.
- configure datadog to monitor mongodb metrics.

## Requirement

This playbook require several software installed in your machine:
- ansible v2.8
- git

## Variables

These variables are required to run this playbook.

    - name: service_name
      description: the service name of this mongodb cluster, this variable is used for configuring log streaming.
      example: tsidata
    - name: replica_set_name
      description: name of the replica set / mongodb cluster, if you are migrating from multi account you need to use the same replica_set_name
      example: tsidatarepl
    - name: mongodb_repl_oplog_size
      description: : size of the mongo oplog in MB
      example: 10000
    - name: hostname
      description: FQDN for mongodb instance
      example: tsidata-mongod-01.example.com
    - name: temporary_master_password
      description: temporary master password for initial root user
      example: don'tusethisunsecuredpassword
    - name: keyfile
      description: keyfile for mongodb cluster between instances to authenticate, by defaul this playbook will lookup the keyfile in `/tmp/keyfile`, you can     generate key file and store it in /tmp/key file by running this command `openssl rand -base64 741 > /tmp/keyfile`
    - name: datadog_api_key
      description: api key for datadog agent, by default this playbook will look at your parameter store with path `/tvlk-secret/shared/<changethiswithyourpd>/datadog.api.key`, you need to make sure this path is exist and contains the correct datadog api key

## How to configure mongodb instance.

This playbook 
- Download this repo to your ansible playbook directory example: /etc/ansible/playbook
```bash
mkdir /etc/ansible/playbook -p
git clone git@github.com:traveloka/ansible-mongodb-3.6-initial-setup.git /etc/ansible/playbook
```
- Download dependency for ansible playbook
```bash
cd /etc/ansible/playbook/ansible-mongodb-3.6-initial-setup
ansible-galaxy install -r requirements.yml
```
- Run ansible playbook and provide the necesarry value for the ansible playbook variable. To know more about variable, you can see variable section. You can add variable direcctly in playbook.yml by modifying the playbook.yml and the variable and value in var section. Or you can directly provide the variable value when you run the ansible playbook
```bash
ansible-playbook playbook.yml -e "variable1=value1" -e "variable2=value2" -e "variable3=value3"
```