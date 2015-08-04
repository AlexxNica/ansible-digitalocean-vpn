# ansible-digitalocean-vpn
Ansible role for creating personal VPN via DigitalOcean.


### Usage
```
./create-vpn
```

Creates a DigitalOcean droplet for use as an OpenVPN server.
Also configures your localhost as a VPN client and activates
the connection via NetworkManager.


If you already use Ansible to manage systems, then you don't
need the `create-vpn` wrapper script, which handles creating
dynamic inventories. Instead, just include the `digitalocean.yml`
playbook in your `site.yml`, or run it directly:

```
ansible-playbook digitalocean.yml
```

If you have NetworkManager installed (particularly `nmcli`), then the VPN connection
will be automatically activated. If you don't have NetworkManager, use the
OpenVPN config file at `openvpn_files/client.ovpn` to import into whatever
you use to manage network settings.

After running the playbook, check that you are really using VPN:

```
curl ipecho.net/plain
```

That command should return two different IP addresses with and without the VPN
connection activated.

## Requirements

* ansible
* DigitalOcean account

You should have Ansible installed. See [below for installation details](#installing-ansible).

### Installing Ansible
If you haven't used Ansible before, you'll need to install it to use this script.
On most platforms, the simplest way is to use pip:

```
pip install --upgrade ansible
```

On Debian/Ubuntu, consider using the PPA, so updates are installed
automatically as part of the system-wide package management:

```
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```

### DigitalOcean API
In order to provision DigitalOcean droplets, you'll need to configure
API access to DigitalOcean. Make sure to use v2 of the API.

Export the following environment variables on the VPN client:

```
DO_API_TOKEN    # v2_api_token_here
DO_SSH_KEY_ID   # integer slug for SSH key saved in DigitalOcean
DO_SSH_KEY_FILE # local path to SSH private key on disk
```

Once you've exported `DO_API_TOKEN`, you can find the other values via the
`digital_ocean.py` plugin shipped with this role:

```
ansible -m digital_ocean -a 'command=ssh list_keys=true'
```

Choose a key from that list and export its id as `DO_SSH_KEY_ID`.
Also export the full path to the private key as `DO_SSH_KEY_FILE`,

### Setting up a DigitalOcean account
If you don't have a DigitalOcean account, you can sign up for one
via [this affiliate link](https://www.digitalocean.com/?refcode=2b67db67a01d)
 and we'll both get $10 credit. (The website is digitalocean.com/signup if you don't want to use an affiliate link.)
Once you sign up, make sure to add an SSH key to your account so you can connect to your droplets.

## Contributing

Pull requests are welcome. Thanks to Timur Batyrshin for the original version.

## License and Author

* Authors:: Timur Batyrshin, Conor Schaefer
* License:: Apache 2.0
