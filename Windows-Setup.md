_While there are many installers built specifically built for Windows users and workarounds provided for when there aren't, your development experience, and the many command line interactions you'll have with the process will generally be better from a Unix-like environment. If your version of Windows supports it, you are highly encouraged to consider enabling the [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/install-win10). Requirements are Windows 10 version 1607 or later, with the 64-bit architecture. If you're using WSL, flip over to the Linux installation guide from here on._

# Elixir
From the language primary website, [elixir-lang.org install page](https://elixir-lang.org/install.html#windows), follow the Download link in the Windows section for the setup.exe installer and follow the prompts.

# Nodejs
From the official Node website [download](https://nodejs.org/en/download/) page, click the Windows icon to download the msi file, double click and follow the bouncing prompts.

# Git
This is open source, so you won't get very far without Git. Head over to the official git-scm website [download](https://git-scm.com/download/win) page for Windows. Be sure to choose the correct version for your system architecture, either 32 or 64 bit, and click the link to download the setup.exe and follow the prompts.

# Docker and Docker Compose
Docker Desktop for Windows is a fully packaged version of the Docker client tools along with a lightweight virtual machine to run the Linux kernel Docker requires. However, the virtualization technology that makes a Linux kernel available on Windows for Docker's container processes is only available for Windows 10 Pro, Education, and Enterprise editions. If you've got one of these versions of the OS, go to the [Docker Hub download](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe) link to download the installer executable and run it to prompt your way to container city.

If you're stuck with Windows 10 Home edition or an older version of Windows, you'll need to download the latest version of Docker's legacy tool for Windows users, [Docker Toolbox](https://github.com/docker/toolbox/releases/download/v18.09.3/DockerToolbox-18.09.3.exe) and run the executable to get it installed and running.

# Kubectl
In order to interact with the deployed platform, either in a local scenario or production deployment, you'll need the Kubernetes command line tool. Click on the [download link](https://storage.googleapis.com/kubernetes-release/release/v1.15.0/bin/windows/amd64/kubectl.exe) to pull down the CLI executable directly from Google's storage servers.

# Helm
Interrogating the Kubernetes cluster with kubectl is great and you can technically do everything you need with that alone if you feel like typing out a lot more yaml files but to get the platform deployment out the door, upgrade it periodically, and roll back in the event of a problem, you'll need Helm as well. Options are limited on Windows so you'll need to go to the Helm project's [Github releases page](https://github.com/helm/helm/releases) to download the tool executable directly.

# Minikube
For doing full end-to-end local development of the platform, you'll need a local Kubernetes environment to work against and deploy to. Docker for Windows now comes with a single node Kubernetes cluster built in, but if you prefer the traditional Minikube, you'll need to get a virtualization program up and running first.

If you have Docker running by now this problem should be solved in one way or another. Basically, if you're running a version compatible with Docker for Windows (Windows 10 64-bit Pro, Enterprise, or Education) then you'll want to enable the native Windows virtualization service [Hyper-V](https://msdn.microsoft.com/en-us/virtualization/hyperv_on_windows/quick_start/walkthrough_install) if you haven't already had to. If your version is older or the Home edition, your best bet is [Virtualbox](https://www.virtualbox.org/wiki/Downloads). Click the link for the Windows setup executable to download and run it for a walkthrough of the installation.

Once your virtualization is set up, follow the link to the [Minikube installer](https://github.com/kubernetes/minikube/releases/latest/download/minikube-installer.exe) to download and walk through the setup there as well.