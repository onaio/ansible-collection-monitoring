# Ansible Collection - onaio.monitoring

This Ansible collection installs and configures Ona's monitoring stack which includes:

1. Graylog
1. Grafana
1. InfluxDB

## Installing the Collection

We recommend adding this collection as a Git submodule to your playbooks repository. Do this by running:

```sh
git submodule add https://github.com/onaio/ansible-collection-monitoring.git collections/ansible_collections/onaio/monitoring
```

Then make sure the git submodule, as well as its submodules are initialized, by running:

```sh
git submodule update --init --recursive
```

The collection has a set of prerequisite pip packages and Ansible roles. To install the prerequisite pip packages, run (while inside this repo's root directory):

```sh
python --version # confirm that your active python version is 3
pip install -r requirements/base.pip
```

To install the prerequisite Ansible roles, run (while inside this repo's root directory):

```sh
ansible-galaxy role install -r requirements/ansible-galaxy.yml -p <the directory to install the roles. Check your ansible.cfg for supported paths in roles_path>
```

## Using the Collection

Inside your inventory, you will need to define the following groups:

1. monitoring_grafana
1. monitoring_graphite
1. monitoring_graylog
1. monitoring_postgresql
1. monitoring_elasticsearch

The different services will be installed in your hosts depending on which groups the hosts are part of.

You will be required to define the following variables:

```yaml
monitoring_sentry_db_password:
monitoring_grafana_db_password:
monitoring_es_graylog_password:
monitoring_graphite_api_influxdb_graphite_pass:
monitoring_sentry_domain:
monitoring_grafana_domain:
monitoring_graylog_domain:
monitoring_grafana_admin_password:
monitoring_grafana_github_client_secret:
monitoring_graphite_api_influxdb_admin_pass:
monitoring_setup_type: "single-host"
monitoring_certbot_mail_address:
monitoring_graylog_password_secret: <Make sure this is at least 20 characters long>
monitoring_graylog_root_password:
```

### SSL certificates

This collection uses SSL certificates from Letsencrypt. You can override this by setting `monitoring_use_certbot` value to `false` and adding the following variables:

```yaml
monitoring_use_certbot: false # Setting this to false, will require we specify the SSL certificate/keys to use
monitoring_ssl_cert_directory: "{{ inventory_dir }}/files/ssl" # the directory to search for the certificate and key
monitoring_ssl_cert: "certificate.pem" # filename of the certificate file
monitoring_ssl_key: "key.pem" # filename of the key file
```

You can include the playbooks defined in this collection in your playbook as illustrated below:

```yaml
---
- import_playbook: collections/ansible_collections/onaio/monitoring/playbooks/postgresql.yml
  tags:
    - postgres
    - postgresql
- import_playbook: collections/ansible_collections/onaio/monitoring/playbooks/graphite.yml
  tags:
    - graphite
    - graphite-api
- import_playbook: collections/ansible_collections/onaio/monitoring/playbooks/grafana.yml
  tags:
    - grafana
- import_playbook: collections/ansible_collections/onaio/monitoring/playbooks/elasticsearch.yml
  tags:
    - es
    - elasticsearch
- import_playbook: collections/ansible_collections/onaio/monitoring/playbooks/graylog.yml
  tags:
    - graylog
```
