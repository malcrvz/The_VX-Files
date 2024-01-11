# Packet management

Linux package managers are tools that automate installation, removal and management of software packages. Packages are archives that contain binaries of software, configuration files, information about dependencies and keep track of updates and upgrades. They go search this packages at repositories. Repositories are servers all around the world that contain the software packages and their metadata, when you download or update, the packet manager uses these repositories, they come by default in your Linux distribution and are associated to it. While technically possible to use packages from other Linux distributions, it's generally not recommended as it can lead to issues and conflicts with library versions, configurations, dependencies, etc. \
If you are most likely using Kali Linux for your cybersecurity studies, you are using a Debian-based distro, and therefore using the APT packet manager.

{% hint style="info" %}
In Debian you can check or modify your repositories at `/etc/apt/sources.list`
{% endhint %}

***

### APT (Advanced Package Manager)

```bash
apt update              #Updates information about the packages avaiable in the repository, "synchronizes" with it
apt upgrade             #Updates the packages you have in your system to the latest version fetched by the "apt update"
apt install abc         #Installs specified package
apt remove abc          #Uninstalls specified package but keeps the configuration files, so it you install it again keeps its old config
apt purge abc           #Uninstalls specified package fully, config files too
apt-cache search abc    #Searchs packets in the APT database downloaded from the repository, useful if we can't remember the name of the package to download
apt-cache show abc      #Shows detailed information on the package
apt list --installed    #Prints a list of all the installed packages in our system

```

***

### DPKG

To install `.deb` Debian packages, we can also use dpkg&#x20;

```bash
dpkg -i abc.deb             #Installs the specified package
wget url/package.deb && dpkg -i package.deb
```

***

### GIT

We can download GitHub packages directly with. By default they will be downloaded to the current directory.

```bash
git clone https://url.git
git clone https://url.git ~/DirectoryToSave
mkdir ~/DirectoryToSave && git clone https://url.git ~/DirectoryToSave

```
