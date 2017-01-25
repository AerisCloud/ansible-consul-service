# Consul Service

Create the configuration file for a consul service.

## Usage

### In playbook

```yaml
  roles:
    - role: consul-service
      consul_service_name: bastion
      consul_service_address: "{{ ec2_ip_address }}"
    - role: consul-service
      consul_service_names: ['web', 'api', 'worker']
      consul_service_name: app
      consul_service_tags: ['staging']
```

### In role dependencies

```yaml
dependencies:
  - role: consul-service
    consul_service_name: ldap
    consul_service_port: 389
    consul_service_tags: ['production']
```

## Variables

* `consul_service_name`, `consul_service_names` (Required) - A string or list of strings with the names of the services.
* `consul_service_address` (Optional) - The IP address used to reach the service. If not defined, the IP address of the node will be used.
* `consul_service_port` (Optional) - The port used to reach the service.
* `consul_service_tags` (Optional) - The tags property is a list of values that are opaque to Consul but can be used to distinguish between primary or secondary nodes, different versions, or any other service level labels.
