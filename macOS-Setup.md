# Elixir
The easiest way to get started with Elixir on macOS is via Homebrew. With Homebrew installed, run the `brew install elixir`

While this will get you up and running quickly, you may also consider installing Elixir and Erlang via the [asdf](https://github.com/asdf-vm/asdf) version manager. This will allow you to have a per-project version of Elixir and allow backward compatibility of versions without enforcing a single global version across your entire machine.

## asdf-vm
asdf version manager is a great tool that will make it easy to manage multiple versions of dozens of languages and CLI tools. It's worth your time to set up. Best of all, it standardizes the version management interface across macOS and Linux.

To install, clone the code directly from the project repository with the command `git clone https://github.com/asdf-vm/asdf.git ~/.asdf` and add the tool to your path by adding the following lines to your `.bashrc` or `.bash_profile`:
`. $HOME/.asdf/asdf.sh`
`. $HOME/.asdf/completions/asdf.bash`

Once asdf is installed you can run `asdf plugin-add <name>` (elixir for instance) to install the language plugin, list all available versions of the language with `asdf list-all <name>`, install the desired version with `asdf install <name> <version>`, and once installed, set the global version of the language with `asdf global <name> <version>` or a project-specific version of the language by changing into the project directory and typing `asdf local <name> <version>`

# Nodejs
Again, the easiest way to install Nodejs is from the project's official [download](https://nodejs.org/en/download/) page. Clicking the icon for the macOS platform will download a `.pkg` file that will open the Nodejs install wizard and walk you through the process.

However, with Node dependency management being one of nature's biggest mistakes, you may seriously want to consider installing and managing it via asdf as well. Just follow the instructions above and install the desired version with `asdf plugin-add nodejs` and `asdf install nodejs <version>`

# Git
This is open source, so you won't get very far without Git. Fortunately, you're on a Mac and despite the default version of Git being a seriously old one, you still have it installed by default. If you'd like to update your version to something a little more modern, you can run `brew install git` to get a newer version from Homebrew.

# Docker and Docker Compose
Docker for Mac is a fully packaged version of the Docker client tools along with a lightweight virtual machine to run the Linux kernel Docker requires (sorry, the proprietary Darwin version of BSD won't cut it). Download the [official package installer](https://download.docker.com/mac/stable/Docker.dmg) from Docker and open the disk image to be walked through the installation prompts.

# Kubectl
In order to interact with the deployed platform, either in a local scenario or production deployment, you'll need the Kubernetes command line tool. You can install this via Homebrew with the command `brew install kubectl` or, once again, asdf is your friend here if you run the commands `asdf plugin-add kubectl` and `asdf install kubectl <version>`.

# Helm
Interrogating the Kubernetes cluster with kubectl is great and you can technically do everything you need with that alone if you feel like typing out a lot more yaml files but to get the platform deployment out the door, upgrade it periodically, and roll back in the event of a problem, you'll need Helm as well. Homebrew will solve this for you with the command `brew install kubernetes-helm` but, as you may have guessed, asdf has you covered here as well with the commands `asdf plugin-add helm` and `asdf install helm <version>`.

# Minikube
For doing full end-to-end local development of the platform, you'll need a local Kubernetes environment to work against and deploy to. Docker for Mac now comes with a single node Kubernetes cluster built in, but if you prefer the traditional Minikube, you can install via Homebrew with the command `brew cask install minikube` or install the binary directly with `curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin`. As always, asdf is also an option.