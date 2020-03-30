# Machine setup

Follow the instructions with the specific operating system for your machine.

Once that's done, follow instructions in the [General](#general) section.

## Windows

**⚠️ You MUST run all installation commands with administrator privileges**

Follow the folowing instructions to run commands with administrator privileges:

- Run [PowerShell with administrator privilege](https://www.thewindowsclub.com/how-to-open-an-elevated-powershell-prompt-in-windows-10)
- Run [Command Prompt with administrator privilege](https://www.howtogeek.com/194041/how-to-open-the-command-prompt-as-administrator-in-windows-8.1/).

### Git for Windows

Download and install [Git for Windows](https://gitforwindows.org/).

### Install Chocolatey package manager

Install Chocolatey by following these [instructions](https://chocolatey.org/install).

### Upgrading outdated packages

If you already have these packages previously installed on your computer, then you would want to make sure they are up to date. If you previously installed them via `choco` then run the following command to upgrade your packages.

```sh
choco upgrade <package-name>
```

### Install Node.js and npm

Install Node.js and npm

```sh
choco install nodejs
```

If node is already installed, upgrade to the latest version of node

```sh
choco upgrade nodejs
```

For more details, see https://chocolatey.org/packages/nodejs.
Alternatively, you can also download nodejs and npm from the [official website](https://nodejs.org/en/).

## Windows Subsystem for Linux (WSL)

If you're on Windows 10, you can use [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/about?redirectedfrom=MSDN), which provides a familiar Bash environment with Unix command line utilities.

- WSL is available only in 64-bit versions of Windows 10 from version 1607. It is also available in Windows Server 2019.

### Update windows to latest version

1. Click on the Home screen button and search for update
2. Select check for updates, this should bring you to the Windows Update page
3. Click on download, go for a break, it will take awhile and require several restart

### Enable Window Subsystem

1. Open powershell with admin privillege
2. Run command `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux`
3. Restart computer

### Install Ubuntu

1. Go to microsoft page to install Ubuntu
2. Initialise a new distro instance
3. Create user with password
4. Remember your password or store in password manager

### Install Node

1. Open Ubuntu on windows
2. Update package manager `sudo apt update && sudo apt upgrade`
3. Update node source `curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -`
4. Install node `sudo apt-get install nodejs`
5. Install npm `sudo apt-get install npm`
6. Check node version `node --version`
7. Check npm version `npm --version`

### Set up VS Code

1. Install VS code
2. Install wsl extension `remote wsl extension`
   `https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl`
3. restart vscode
4. Open settings.json (ctrl + p, type `settings.json`)
5. Add this line `Bash on Ubuntu (on Windows) "terminal.integrated.shell.windows": "C:\\Windows\\System32\\bash.exe"`

## Mac

### Install Homebrew package manager

Open Terminal app and run the following command.

```sh
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Next, check that Homebrew was installed properly. Fix any issues when prompted.

```sh
brew doctor
```

### Install Git

Install Git

```sh
brew install git
```

### Install Node.js and npm

Install Node.js via Homebrew. npm will be installed together with Node.js.

```sh
brew install node
```

### Upgrading outdated packages

If you already have these packages previously installed on your computer, then you would want to make sure they are up to date. If you previously installed them via `brew` then run the following command to upgrade your packages.

```sh
brew upgrade <package-name>
```

## Linux

### Package manager

Check which distribution of Linux you are using

```sh
lsb_release -a
```

Your package manager will differ depending on your Linux distribution.

For Debian based distributions (e.g. Ubuntu) you will use `apt`.

For Fedora based distributions (e.g. RHEL, CentOS) you will use `dnf`. Note that `dnf` is the next-generation version of `yum`. To install `dnf` run `yum install dnf`.

### Install Git

For Debian based distributions

```sh
sudo apt install git-all
```

For Fedora based distributions

```sh
sudo dnf install git-all
```

### Install Node.js

Official Node.js [binary distributions](https://github.com/nodesource/distributions) for various Linux distributions are provided by NodeSource.

Refer to installation instructions for [Fedora based distributions](https://github.com/nodesource/distributions/blob/master/README.md#rpminstall).

Refer to installation instructions for [Debian based distributions](https://github.com/nodesource/distributions/blob/master/README.md#debinstall).

### Upgrading outdated packages

Run the following command to upgrade outdated packages for your respective Linux distro.

```sh
sudo apt install --only-upgrade <package-name>
```

```sh
sudo dnf upgrade <package-name>
```

## General

Follow these instructions for all operating systems

- Download Slack and join the Jumpstart Slack workspace (please ask instructors for invite link)
- Create a GitHub account (if you don't have one already)
- Install Visual Studio Code for your operating system
- Install Firefox web browser for your operating system
- Install Google Chrome web browser for your operating system

### Install Visual Studio Code `code` command in PATH

- Open the command palette (Mac: Cmd+Shift+P; Windows: Ctrl+Shift+P or just press F1.)
- Type `shell` and select `Shell Command: Install 'code' command in PATH`.

![code](../_media/code.png)

You can now use the `code` command to open Visual Studio Code.

To open the entire folder / directory:

```
code .
```

### Install Visual Studio Code extensions

Install these useful Visual Studio Code [extensions](/miscellaneous/resources?id=vs-code-extenstions)

### Install Node version manager (optional)

We often need to use different versions of Node on different projects. The easiest way to manage different versions of Node for multiple projects on your computer, is to use a version manager like [`n`](https://github.com/tj/n).

```
npm install --global n
```

Verify `n` was installed.

```
n --version
```

### Install MongoDB

Find the instructions for your specific OS and version for the [Community Edition](https://docs.mongodb.com/manual/installation/).

If you're running Windows, please add the MongoDB bin folder to your System PATH (e.g. C:\Program Files\MongoDB\Server\4.0\bin) so that the `mongo` command will be able to work in the terminal.

### Install a Visual Studio Code extension for viewing data in MongoDB

- Open the Extensions pane in Visual Studio Code
- Search for the `Azure Cosmos DB extension` by Microsoft
- Install it and reload VSCode once the installation completes
- Click the new Azure icon on the left sidebar
- Create a new MongoDB connection by going to `Cosmos DB` and then clicking the 'plug' icon and selecting `MongoDB` from the dropdown
- Make sure to start the server before connecting to it

Alternatively, use [MongoDB Compass](https://www.mongodb.com/products/compass).

### Configuring Git

To attach your full name to every commit you, simply add this line (of course, change it to your own name):

```sh
git config --global user.name "Jane Doe"
```

You can keep your email address private by using `<username>@users.noreply.github.com`. Just replace `<username>` with your actual GitHub username (e.g. `janedoe@users.noreply.github.com`)

```sh
git config --global user.email "<username>@users.noreply.github.com"
```

Add the following optional, but recommended configurations:

```sh
git config --global push.default simple
git config --global credential.helper cache
git config --global core.autocrlf input
git config --global pull.rebase true
git config --global rebase.autoStash true
git config --global core.editor 'code --wait'
```

### Verify installations

Verify your packages have been installed correctly

```sh
git --version
which node
node --version
which npm
npm --version
code --version
mongod --version
```
