# Machine setup

Follow the instructions in the General section and then continue on with the specific operating system for your machine.

## General

- Download Slack and join the JumpStart Slack workspace
- Create a GitHub account
- Install Visual Studio Code
- Install Google Chrome web browser

### Visual Studio Code extensions

Here is a list of useful extensions that you should install

- [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [Auto Rename Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag)
- [Auto Close Tag](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-close-tag)
- [Jest](https://marketplace.visualstudio.com/items?itemName=Orta.vscode-jest)
- [YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml)

## Windows


## Mac

If you already have these packages previously installed on your computer, then you would want to make sure they are up to date. To upgrade your packages, run the following command.

```sh
brew upgrade <package-name>
```

### Install Homebrew

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

Verify

```
git --version
```

### Install Node.js and npm

Install Node.js via Homebrew. npm will be installed together with Node.js.

```sh
brew install node
```

Verify Node.js was installed.

```sh
node --version
```

Verify npm was installed.

```sh
npm --version
```

### Install Node version manager

We often need to use different versions of Node on different projects. The easiest way to manage different versions of Node for multiple projects on your computer, is to use a version manager like [`n`](https://github.com/tj/n).

```
npm install --global n
```

Verify

```
n --version
```

## Linux
