# Consul Service

Create the configuration file for a consul service.

## Usage

### In playbook

```yaml
  roles:
    - role: consul-service
      consul_service_name: bastion
      consul_service_address: "{{ ec2_ip_address }}"
```

### In role dependencies

```yaml
dependencies:
  - role: consul-service
    consul_service_name: ldap
    consul_service_port: 389
```

## Variables

* `consul_service_name` (Required) - The name of the service.
* `consul_service_address` (Optional) - The IP address used to reach the service. If not defined, the IP address of the node will be used.
* `consul_service_port` (Optional) - The port used to reach the service.
