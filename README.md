# overlay_demo

This is a simple demonstration of a overlay network created by using [tailscale](https://tailscale.com/) and [headscale](https://github.com/juanfont/headscale).
It uses Vagrant to create a system consisting of nodes, each behind a NAT server, and one controller node.

### Network layout

![Class Diagram](https://www.plantuml.com/plantuml/proxy?src=https://raw.githubusercontent.com/Tideless/overlay_demo/master/diagrams/network.puml)

## Prerequisites

- [Vagrant](http://vagrantup.com)
- [Vagrant-hosts](https://github.com/oscar-stack/vagrant-hosts)
