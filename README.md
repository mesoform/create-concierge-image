# Role to create a Concierge managed image 

## Using this role

Generally this role should be wrapped by a [top level playbook for generating application 
config](https://github.com/mesoform/concierge-app-playbook) to be built into the image.

If ran manually, there is a `concierge-image.yml` file which can be passed to `ansible-playbook` but many of the required variables
and files will need to be set up locally.

Primarily the role simply generates a Dockerfile and builds an image. There is a template in the template/build directory but any 
file in this directory with a .j2 suffix will be processed.

The docker file has some default attributes set and allows for others to be included by creating the required lists or variables.

Currently these are as follows:

* `install_script` (string) = the location of the script required to install the application you want to package into the image
* `build_args` (list) = a list of additional Docker ARG options for required variables when building
* `container_vars` (list) = environment variables, local to the container image.
* `labels` (list) = a list of additional labels to add to the container 
* `ports` (list) =  a list of ports to expose
* `volumes` (list) = a list of volume the container should create
* `entrypoint` (string) = process or script to run as `ENTRYPOINT`. For the concierge managed containers, it is assumed that unless 
you're creating a base image, this will always be containerpilot and already set in the base image
* `command` (string) = process or script to run as CMD. For the concierge containers, it is assumed that generally this will be 
passed via orchestration files like docker-compose.yml

## Testing
For basic testing, add a test tag to the tasks you want to test and run `ansible-playbook -vv concierge-image.yml -t test`.

However, because it's generally unlikely this role will be used on its own much, and because it is so easy, we recommend
 [testing whilst integrated as a submodule](https://github.com/mesoform/concierge-app-playbook/blob/master/README.md#submodules)

`-----------------------------------------------------------------------------------------------------------------`
Template by [Mesoform Limited](http://www.mesoform.com)