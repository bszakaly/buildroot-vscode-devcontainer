# buildroot-vscode-devcontainer
Devcontainer config for use in vscode for Buildroot builds in a container.

# Configuration

## Configure container user & group

In the `Dockerfile`, I set up an `ubuntu` user that matches my host system's user & group ID. The `Dockerfile` configuration will work out of the box if your `UID:GID` is `1000:1000` on your host system. To check your IDs, you can do:
```
id -u
id -g
```

Otherwise, export your `UID` and `GID` in your `.bashrc`. Your user ID is set in the `UID` environment variable by default on an Ubuntu system. You do need to set your own group ID in an environment variable manually, so it can be read in the `Dockerfile`. In your `~/.bashrc`, add at the end:
```
export UID
export GID=$(id -g)
```

## Ubuntu version

You might want to change the `Ubuntu` version in the `Dockerfile` depending on which version of `Buildroot` you are using. Some combinations work out of the box, others don't. I tested building with buildroot-2021.02 with an Ubuntu 20.04 container image.

## Additional packages for Buildroot

Depending on your configuration, you might need additional packages installed for your Buildroot build. For example, a different source fetching tool should be installed in the `Dockerfile` if needed. Packages that are specifically required to build for your target platform should be included in your target platform's `defconfig` file (option `BR2_PACKAGE_HOST_?`).

## gcc-?-plugin-dev

You need to install the correct `gcc-?-plugin-dev` package according to what `GCC` version your platform's `defconfig` file specifies in Buildroot. This is set in the `BR2_GCC_VERSION_?_X` option. For example, if your target platform's `defconfig` specifies `BR2_GCC_VERSION_10_X=y`, make sure the `gcc-10-plugin-dev` package is installed in the `Dockerfile`. I included multiple versions in there by default.

## vscode plugins in the devcontainer

You might want to add a `extensions` section in `devcontainer.json`, so you can use the required extensions for your work in the devcontainer.

## Mounted files & directories

The following are mounted into the container:
- `SSH_AUTH_SOCK`: allows you to use your SSH keys that are on your host system for cloning private packages in your Buildroot build
- `.gitconfig`: allows you to use your identity on your host system with `git` inside the devcontainer e.g. for commits to your Buildroot repository fork
- `buildroot` directory: the Buildroot directory that you manually need to clone/copy into this directory

# Setup

Make sure you have the prerequisites that are listed in the [Visual Studio Code Guide](https://code.visualstudio.com/docs/devcontainers/containers).

Clone the [Buildroot repository](https://git.buildroot.net/buildroot/) (probably you want to clone your fork/private version with your custom config instead) in this directory.
```
git clone git://git.busybox.net/buildroot
```

Open this directory in `vscode` and bring up the command palette (default Ctrl+Shift+P). Select `Dev Containers: Reopen in Container`.

You can now start working with Buildroot; for example to build the `raspberrypi4_defconfig` with Buildroot version `2021.02`:
```
git checkout 2021.02
make raspberrypi4_defconfig && make
```