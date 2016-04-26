# vagrant_chef-delivery
Simple demo of CHEF Delivery deployable with vagrant

This Vagrantfile can be used to bring up a basis demo of CHEFs Delivery Server.
It implements the [documentation](https://docs.chef.io/install_delivery_ssh.html) provided by CHEF

## Prerequisites:

* installed vagrant and virtualbox
* a beefy machine (min 10gb RAM)
* ssh key pair (id_rsa & id_rsa.pub)
* delivery.license provided by CHEF
* test.json (sample included) - created by 'rake setup:generate_env'

-> place ssh keys, license and an adapted (optional) test.json beside the Vagrantfile

## Usage

run via ´vagrant up´

## Contributors

Sebastian Sucker <sebastian@endocode.com>
Christian Johannsen <christian@chef.io>
