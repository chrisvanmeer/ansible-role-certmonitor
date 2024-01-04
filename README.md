ansible-role-certmonitor
========================

A role to monitor the expiration of any found certificate on any host.

Requirements
------------

No requirements.

Role Variables
--------------

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yml
certmonitor_include_paths_global:
  - /etc/pki
  - /etc/ssl
  - /opt
```

The default paths we are going to check. Will be merged with the `_group` and `_host` variable.

```yml
certmonitor_include_paths_group: []
```

Optional addition of paths on a group level.

```yml
certmonitor_include_paths_host: []
```

Optional addition of paths on a host level.

```yml
certmonitor_include_patterns_global:
  - '.*\.crt$'
  - '.*\.pem$'
```

These regex patterns will be checked for filenames. Will be merged with the `_group` and `_host` variable.

```yml
certmonitor_include_patterns_group: []
```

Optional addition of patterns on a group level.

```yml
certmonitor_include_patterns_host: []
```

Optional addition of patterns on a host level.

```yml
certmonitor_exclude_patterns_global:
  - '/etc/pki/product-default/.*\.pem$'
  - '/etc/pki/ca-trust/extracted/openssl/ca-bundle.trust.crt'
  - '/etc/pki/ca-trust/extracted/pem/.*\.pem$'
  - '/etc/pki/fwupd.*\.pem$'
  - '/etc/pki/consumer/.*\.pem$'
  - '/etc/pki/entitlement/.*\.pem$'
  - '/etc/pki/nginx/dhparam.pem'
  - '/etc/pki/tls/certs/localhost.crt'
  - '.*-key\.pem$'
  - '.*\.key\.pem$'
```

Regex patterns that we will exclude from inspection. Mainly default certificates and private keys. Will be merged with the `_group` and `_host` variable.

```yml
certmonitor_exclude_patterns_group: []
```

Optional addition of exclusion patterns on a group level.

```yml
certmonitor_exclude_patterns_host: []
```

Optional addition of exclusion patterns on a host level.

```yml
certmonitor_validity_check: "+2w"
```

The check of the certificates validity specified in weeks from now. By default we use "+2w" to report certificates that expire within the next two weeks.

```yml
certmonitor_email_enabled: false
```

Default email reporting is disabled, set this to `true` to enable.

```yml
certmonitor_email_subject: "Expiring TLS Certificates"
```

Email subject when sending email reports.

```yml
certmonitor_email_subtype: "html"
```

Setting the email mime type to `html`. Can also be set to `plain`. One can modify the template to their choosing to match this.

There are more variables available for the email section. Refer to the last task in the playbook for this. If not existent, these will be omitted, but this gives you the option to include these values in variables, rather than having to edit the playbook.

Dependencies
------------

For the certificate inspection, this role depends on the `community.crypto.x509_certificate_info` module.  
For email, this role depends on the `community.general.mail` module.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yml
- name: Certificate Monitoring
  hosts: all
  become: true

  vars:
    certmonitor_email_enabled: true
    certmonitor_email_subject: "Expiring TLS Certificates"
    certmonitor_email_sender: "certificates@yourdomain.com"
    certmonitor_email_recipient: "soc@yourdomain.com"
    certmonitor_smtp_server: "smtp.yourdomain.com"
    certmonitor_smtp_port: 25

  roles:
     - role: chrisvanmeer.certmonitor
```

License
-------

BSD

Author Information
------------------

- Chris van Meer <c.v.meer@atcomputing.nl>
