Description
===========

This is a template for deploying a Django application on multiple Linux servers
with [OpenStack Heat](https://wiki.openstack.org/wiki/Heat) on the [Rackspace
Cloud](http://www.rackspace.com/cloud/). A Load Balancer and Cloud Database
will also be deployed. This template is leveraging
[chef-solo](http://docs.opscode.com/chef_solo.html) to set up the server.

Requirements
============
* A Heat provider that supports the Rackspace `OS::Heat::ChefSolo` plugin.
* An OpenStack username, password, and tenant id.
* [python-heatclient](https://github.com/openstack/python-heatclient)
`>= v0.2.8`:

```bash
pip install python-heatclient
```

We recommend installing the client within a [Python virtual
environment](http://www.virtualenv.org/).

Example Usage
=============
Here is an example of how to deploy this template using the
[python-heatclient](https://github.com/openstack/python-heatclient):

```
heat --os-username <OS-USERNAME> --os-password <OS-PASSWORD> --os-tenant-id \
  <TENANT-ID> --os-auth-url https://identity.api.rackspacecloud.com/v2.0/ \
  stack-create djangoapp -f django_multi.yaml \
  -P project_name=myproject -P app_name=myapp
```

* For UK customers, use `https://lon.identity.api.rackspacecloud.com/v2.0/` as
the `--os-auth-url`.

Optionally, set environment variables to avoid needing to provide these
values every time a call is made:

```
export OS_USERNAME=<USERNAME>
export OS_PASSWORD=<PASSWORD>
export OS_TENANT_ID=<TENANT-ID>
export OS_AUTH_URL=<AUTH-URL>
```

Parameters
==========
Parameters can be replaced with your own values when standing up a stack. Use
the `-P` flag to specify a custom parameter.

* `venv_username`: The system user to use for a Python virtualenv.
* `app_name`: The name of your application.
* `flavor`: cloud server size to use. (Default: 4 GB Performance)
* `virtualenv`: The location of the Python virtualenv (Default: /srv)
* `project_name`: The name of your Django project.
* `django_admin_user`: The admin username to login to Django.
* `django_admin_email`: The email address for the Django admin user.
* `django_admin_pass`: The password for the Django admin user.
* `db_user`: The MySQL database user for your Django application.
* `db_pass`: The MySQL database password for your Django application.
* `ssh_keypair_name`: Name of the SSH key pair to register with nova (Default:
  none)

Outputs
=======
Once a stack comes online, use `heat output-list` to see all available outputs.
Use `heat output-show <OUTPUT NAME>` to get the value fo a specific output.

* `private_key`: SSH private that can be used to login as root to the server(s).
* `load_balancer_ip`: Public IP address of the Cloud Load Balancer
* `DB ID`: The Instance ID of the Cloud Database

For multi-line values, the response will come in an escaped form. To get rid of
the escapes, use `echo -e '<STRING>' > file.txt`. For vim users, a substitution
can be done within a file using `%s/\\n/\r/g`.

Stack Details
=============
By default the application will be deployed under /var/www/application

Contributing
============
There are substantial changes still happening within the [OpenStack
Heat](https://wiki.openstack.org/wiki/Heat) project. Template contribution
guidelines will be drafted in the near future.

License
=======
```
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
