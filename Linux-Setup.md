# asdf-vm
asdf version manager is a great tool that will make it easy to manage multiple versions of dozens of languages and CLI tools. It's worth your time to set up. Best of all, it standardizes the version management interface across macOS and Linux.

To install, clone the code directly from the project repository with the command `git clone https://github.com/asdf-vm/asdf.git ~/.asdf` and add the tool to your path by adding the following lines to your `.bashrc` file: `source $HOME/.asdf/asdf.sh` and `source $HOME/.asdf/completions/asdf.bash`

Once asdf is installed you can run `asdf plugin-add <name>` (elixir for instance) to install the language plugin, list all available versions of the language with `asdf list-all <name>`, install the desired version with `asdf install <name> <version>`, and once installed, set the global version of the language with `asdf global <name> <version>` or a project-specific version of the language by changing into the project directory and typing `asdf local <name> <version>`

Add the following dependencies to your system as well to ensure asdf behaves as expected. For Debian/Ubuntu-based systems run the command `sudo apt install automake autoconf libreadline-dev libncurses-dev libssl-dev libyaml-dev libxslt-dev libffi-dev libtool unixodbc-de unzip curl` and for Red Hat-based systems, the command `sudo dnf install automake autoconf readline-devel ncurses-devel openssl-devel libyaml-devel libxslt-devel libffi-devel libtool unixODBC-devel unzip curl` is your friend.

# Elixir
For RHEL-flavored Linux, older versions can install Elixir and Erlang via the command `yum install elixir` while newer versions will want `dns install elixir`.

If you're running a Debian-flavor of Linux you'll need to do a little more legwork and first add the deb package to your sources via `wget https://packages.erlang-solutions.com/erlang-solutions_1.0_all.deb && sudo dpkg -i erlang-solutions_1.0_all.deb`, update your dependencies with `sudo apt-get update` and install Erlang and Elixir with the separately (with the same command) `sudo apt install esl-erlang && sudo apt install elixir`

asdf is also an option and allows much more granular (and easier) management of the different available versions of both Erlang and Elixir.

# Nodejs
For older RHEL-flavored Linux, update your source repositories with `sudo yum install epel-release` followed by `sudo yum install nodejs npm`. Newer versions can cut right to the chase with `sudo dnf install nodejs npm`.

Debian-based users will have the language with the single command `sudo apt install nodejs npm`

Again, asdf is an option you should consider as well.

# Git
Most versions of Linux ship with Git anymore but if for some reason yours doesn't install it on Fedora/RHEL systems with `sudo dns install git-all` and on Debian/Ubuntu systems with `sudo apt install git-all`.

# Docker and Docker Compose
First, if you've got a previous version of Docker on your system and want or need to upgrade it, you'll need to uninstall all of the old components.

For the newest versions of the RHEL family, install the core plugins package with `sudo dnf -y install dnf-plugins-core` and configure the stable package repository for Docker with `sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo`. Finally, install the Docker Community Edition with `sudo dnf install docker-ce docker-ce-cli containerd.io`

For older RHEL family members, install some dependent packages for `sudo yum install -y yum-utils device-mapper-persistent-data lvm2` Docker needs for volume management and then configure the Docker stable repository with `sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo`. Finally, install Docker-CE with the command `sudo yum install docker-ce docker-ce-cli containerd.io`

Debian and Ubuntu Linux are unusually bifurcated in this process with different package sources and dependent packages. For Debian, install the dependencies needed with `sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common` and add Docker's GPG key with `curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -`. Add the package repository to your apt sources with `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"` and perform one more update before installing Docker via `sudo apt-get install docker-ce docker-ce-cli containerd.io`.

Finally with Ubuntu, install the very similar dependencies `sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common` and add Docker's GPG key, `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`. Install the new apt source with `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`, update once more and finally install via `sudo apt-get install docker-ce docker-ce-cli containerd.io`

Docker Compose is much easier: for any of the supported distros, run the command `sudo curl -L "https://github.com/docker/compose/releases/download/1.23.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`. Make it executable and move it into your path.

# Kubectl
In order to interact with the deployed platform, either in a local scenario or production deployment, you'll need the Kubernetes command line tool. There are native packages available, but for a simple compiled Go binary, why go to all that trouble? Simply run the command `curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl` to pull the latest stable release direct from Google storage servers. Make it executable and move it into the preferred directory in your path.

You can also throw in with asdf here once again via `asdf plugin-add kubectl` and `asdf install kubectl <version>`.

# Helm
Interrogating the Kubernetes cluster with kubectl is great and you can technically do everything you need with that alone if you feel like typing out a lot more yaml files but to get the platform deployment out the door, upgrade it periodically, and roll back in the event of a problem, you'll need Helm as well. The best options are to download directly from the Helm project's [Github releases page](https://github.com/helm/helm/releases) or to use, wait for it...asdf once again. `asdf plugin-add helm` and `asdf install helm <version>`.

# Minikube
For doing full end-to-end local development of the platform, you'll need a local Kubernetes environment to work against and deploy to. If you don't already have a hypervisor for virtual machine management on your system, install [KVM](https://www.linux-kvm.org/) or [VirtualBox](https://www.virtualbox.org/wiki/Downloads). Once that's complete, download the minikube binary directly from Google storage `curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64` and make it executable before moving it into your path.

Alternatively, asdf for the win once again. You know the drill by now.