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
* `consul_service_checks` (Optional) - A list of checks that matches the description below

## Checks

Most fields are optional and have sane defaults, though you do want to define a type or your check won't do a thing.
Each check has one or more mandatory fields (usually named like the check type), see the examples below:

```yaml
consul_service_checks:
  - id: web-http-check
    name: "Checks for /_ping on port"
    type: http
    # required, the URL to ping, checks the return code (2xx for pass, 429 warning, anything else error)
    http: "http://{{ consul_service_address }}:{{ consul_service_port }}/_ping"
  - id: web-tcp-check
    name: "Checks for TCP connection on port"
    type: tcp
    # optional, defaults to localhost:{{ consul_service_port }}
    tcp: "{{ consul_service_address }}:{{ consul_service_port }}"
  - id: web-script-check
    name: "Runs script and checks the exit code"
    type: script
    # required, cannot take arguments
    script: "/usr/local/bin/myscript"
  - id: web-ttl-check
    name: "Stays up for as long as the TTL value, needs to be pinged through the HTTP api"
    type: ttl
    # required, takes a human duration
    ttl: "5m"
  - id: web-docker-check
    name: "Does a docker exec in a container and checks the exit code"
    type: docker
    # required, the ID of the container to run the check in
    docker_container_id: "1d88ad189f4"
    shell: "/bin/bash"
    script: "/app/local-check.py"
```

The following variables are also available:

* `interval` - The interval between each check (N/A for TTL)
* `timeout` - How much time the check has to answer (N/A for TTL)
* `deregister_critical_service_after` - Deregister service after it has been marked as critical for the given amount of time