# Setup a development environment in a container

This project uses ansible to build containers and images - through [buildah](https://github.com/containers/buildah) - that are ultimately
a development environment.

This was started because on the [Fedora Silverblue OS](https://silverblue.fedoraproject.org/) one is supposed to use containers for everything and install very little on the actual OS.

So, borrowing a lot from [Tomas Tomecek's excellent blog](https://blog.tomecek.net/post/building-containers-with-buildah-and-ansible/) I set out to build my own development environment in a highly reproduce-able way.

From the top level directory run this command

`ansible-playbook main.yml --extra-vars=@config.yml`

to build the image.

## Things to reconfigure
Be sure to take a look at [config.yml](https://github.com/johrstrom/dev-env/blob/master/config.yml) and reconfigure to what you want.  There are a lot of configurations that are specific to me like `image_name` because that defaults to `jeffo-dev-env` which is perhaps not the image name that you want.

## Roles
I could never get handler sharing across plays to work right, so I had to divide everything out into roles even though there's not much to any of them.

`container` -- this role configures the container

`container-ctrl` -- this role actually pulls and tags images. The other roles rely on it's handlers.

`image` -- this role installs rpms and makes users and does all the OS level stuff on the container


## When the playbook fails
If the playbook fails, the next run of the playbook will correctly handle the error of using a container that's already in use. 

or the hard way directly removing the containers.
`buildah rm tmp/dev-wrk` if you've replace the variable `working_container` then the command will be

`build rm <container_name>` replacing container_name here with what you've provided.
