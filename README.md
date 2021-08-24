# Ansible Role: PostgreSQL

An Ansible role, which installs and manages an Envoy Docker container.

## Requirements

This role doesn't install Docker on the host. You need to make sure the docker
is installed and running on your system. You can do that e.g. with [ericsysmin's collection](https://galaxy.ansible.com/ericsysmin/docker) (which only contains the docker role).

## Role Variables

| Variable                           | Default Value                           | Description                                                                         |
| ---------------------------------- | --------------------------------------- | ----------------------------------------------------------------------------------- |
| `envoy_docker_image`               | envoyproxy/envoy-distroless             | The docker image, which is used to pull the container.                              |
| `envoy_docker_image_tag`           | v1.19-latest                            | The tag of the image.                                                               |
| `envoy_container_name`             | envoy                                   | The container name.                                                                 |
| `envoy_port`                       | 80                                      | The http port ansible will make available to the host.                              |
| `envoy_port_ssl`                   | 443                                     | The https port ansible will make available to the host.                             |
| `envoy_networks`                   | `- name: "{{ docker_network_name }}"`   | The network the container will be connected to.                                     |
| `envoy_container_memory_limit`     | null                                    | A memory limit for the container                                                    |
| `envoy_upgrade`                    | no                                      | When set to `yes` the container will be updated to the latest version (of the tag). |
| `envoy_config`                     | null                                    | The envoy configuration                                                             |

### Envoy config

You can put your entire configuration in ´envoy_config`:

```yaml
---

envoy_config:  # A variable of this Ansible role
  static_resources:
    listeners:
      - name: listener_http
        address:
          socket_address: { address: "::", port_value: 8080, ipv4_compat: true }
        filter_chains:
          [...]
      - name: listener_https
        address:
          socket_address: { address: "::", port_value: 4443, ipv4_compat: true }
        listener_filters:
          [...]
```

The only difference are the ports used by this role. Currently it supports the
ports `8080` which gets mapped to `envoy_port` (default: `80`) and `4443` which 
gets mapped to `envoy_port_ssl` (default: `443`).

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

```yaml
---

- hosts: envoy
  become: true
  roles:
    - michaelsasser.envoy
```

## License

Copyright &copy; 2021 Michael Sasser <Info@MichaelSasser.org>. Released under
the GPLv3 license.
