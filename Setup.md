# Prepare your workstation
In order to test and extend the Smart Cities platform, you'll need to install a few tools on your workstation.
While you may not need all of the following, end-to-end development of the platform will require each of these at some point.
* __Elixir__ - custom applications written for the project
* __Nodejs__ - the front end user interface, using the Reactjs framework
* __Git__ - source code management; cloning the projects and pushing them back up for review
* __Docker and Docker Compose__ - local development and testing; application packaging and deployment
* __Kubectl__ - Kubernetes cluster management and application debugging
* __Helm__ - Application packaging and deployment configuration management
* __Minikube__ - Local kubernetes development testing

# Installation Guide for Mac

This will install `asdf` to get `erlang` and `elixir`. Also docker and the right node version, managed by nvm.

- Make sure you have ssh configured on your machine and github account for cloning github repos over ssh

  - [https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

- `brew install autoconf@2.69`

Alternatively:

```jsx
curl -O http://ftp.gnu.org/gnu/autoconf/autoconf-2.69.tar.gz
tar zxvf autoconf-2.69.tar.gz
cd autoconf-2.69
./configure && make && sudo make install
```

- `brew install asdf`

  - A version manager for multiple programming languages, we'll use it for elixir and erlang
  - If any messages about JDK pop up, try installing JDK with `asdf` , or manually with provided JDK package from Oracle: [https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)

The erlang and elixir versions here might be out of date, check the .tool-versions file for which versions to install. https://github.com/UrbanOS-Public/smartcitiesdata/blob/master/.tool-versions

The versions there support Windows, Mac M1, and Mac Intel.


  - `asdf plugin add elixir`
  - `asdf plugin add erlang`
  - `asdf install erlang 23.2.7.5`
  - `asdf install elixir 1.10.4-otp-23`
  - `asdf global erlang 23.2.7.5` will shim your version for global usage.
  - `asdf global elixir 1.10.4-otp-23` will shim your version for global usage.
  - Afterwards, `iex` should bring up the elixir shell
 
- Clone [https://github.com/UrbanOS-Public/smartcitiesdata](https://github.com/UrbanOS-Public/smartcitiesdata)
- Run `mix deps.get` from the root directory to install the needed dependencies for the entire umbrella
- (Optional) Install node version 10 (latest version is fine) in order to work on [https://github.com/UrbanOS-Public/react_discovery_ui](https://github.com/UrbanOS-Public/react_discovery_ui) and one other repo
  - Consider using nvm to manage node versions, or asdf with a node plugin
  - `nvm install 10`
- Install docker [https://docs.docker.com/docker-for-mac/install/](https://docs.docker.com/docker-for-mac/install/)

Optional but highly recommended: VSCode elixir setup! https://github.com/UrbanOS-Public/smartcitiesdata/wiki/VSCode-Elixir-Setup

# Installation for Windows (Windows Linux Subsystem)

There might be more windows friendly workflows, we'd love suggestions if you have them!

Steps could probably be utilized for an Ubuntu setup as well.

What is WSL (Windows Linux Subsystem?) https://www.youtube.com/watch?v=48k317kOxqg

- Install this WSL Kernal prerequsite. (might be an optional step since not using WSL2? Try to install anyway) https://docs.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package
- Install ubuntu for windows, install this app, launch it, and make a linux user account when prompted. https://apps.microsoft.com/store/detail/ubuntu/9PDXGNCFSCZV?hl=en-us&gl=US

Note: To copy from this terminal window, highlight text, and right click to copy. To paste, right click with nothing highlighted, and text will be placed at the position of your cursor.

Note: To view the files from this linux terminal in windows file explorer, type `explorer.exe .`

### A: Installing ASDF
1. `git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.10.0`
2. `echo ". $HOME/.asdf/asdf.sh" >> ~/.bashrc`
3. `source ~/.bashrc`
4. `asdf plugin add elixir`
5. `asdf plugin add erlang`

### B: Install Erlang + Elixir
1. `sudo apt-get update`
2. `sudo apt-get install libssl-dev make automake autoconf libncurses5-dev gcc unzip`
3. `asdf install erlang 22.3.4.19`
4. `asdf install elixir 1.10.4-otp-22`
5. `asdf global erlang 22.3.4.19` will shim 21.3.8 for global usage.
6. `asdf global elixir 1.10.4-otp-22` will shim 1.10.4 for global usage.
Afterwards, `iex` should bring up the elixir shell

### C: Setting up Docker
1. https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly

### D: Cloning our application
1.   https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
2.   `git clone git@github.com:UrbanOS-Public/smartcitiesdata.git`

- How VSCode talks to the source files stored in WSL land: https://code.visualstudio.com/docs/remote/wsl

## FAQ:

### SSL Errors

( Feb 10 2022 )
After installing everything and attempting to do a `mix deps.get`, if you get
any errors related to SSL, uninstall erlang, and reinstall with the following
env variable set.

There's some weird mac hiccups where openssl is not available to erlang when
erlang is being compiled by kerl. [asdf erlang Dealing with OpenSSL issues on macOS](https://github.com/asdf-vm/asdf-erlang#dealing-with-openssl-issues-on-macos)

First make sure `brew --prefix openssl@1.1` returns a filepath to a valid openssl
install. If not, run `brew install openssl`, and try again.

```
asdf uninstall erlang <version>
export KERL_CONFIGURE_OPTIONS="--without-javac --with-ssl=$(brew --prefix openssl@1.1)"
asdf install erlang <version>
```