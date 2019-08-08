= Playbook to automate upgrade of Tower =
Eric Lavarde <elavarde@redhat.com>
v1.2, 2019-08-08

It's a rather simple role to automate the upgrade of a Tower. It changed slightly over time but is known to have worked from 3.0.2 to 3.2.2, and from 3.2.1 to 3.2.5 to 3.3.0, and then up to 3.3.4, 3.4.2 and 3.5.1.

The steps to use it are relatively simple:

. `cp tower-setup-inventory.example.yml tower-setup-inventory.local.yml`
. adapt this new inventory file to your needs
. call the playbook `tower-setup.yml`, e.g. with something like `ansible-playbook -i tower-setup-inventory.local.yml tower-setup.yml -v`

Check the README.md file of the role for more information and more variables to use to steer the role.

NOTE: the temporary directory isn't cleaned, so you might want to do it once everything looks fine.
