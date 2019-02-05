# Install Node.js

Installing Node.js is a straightforward process for most systems. On any platform, you can install Node.js using one of their pre-built installers available at [http://nodejs.org/](http://nodejs.org).

**Please Note:** Mac users should use Homebrew to install Node.js. DO NOT use the Node.js installer on a Mac unless you know what you're doing. See below.

To install Node.js, simply download the installation package for your platform and install it like any other application.

However, for each operating system, you may consider an additional or alternative process.

## Windows Installation

Windows users should navigate to the [Node.js download page ](https://nodejs.org/en/download/)and choose the 64-bit Windows installer.  Once this is downloaded they can open it \(click on it\) to run the install.

## Mac Installation

Mac users should use [Homebrew](http://brew.sh) to manage packages like Node.js \(and many other development tools\). Homebrew is a popular package management tool for Mac and it makes installing many packages much easier. The Homebrew website \([http://brew.sh](http://brew.sh)\) has a great installation tutorial to follow. Once you've installed Homebrew on your system, you can install Node.js with one command:

```
brew install node
```

Once that command completes successfully, you should have Node.js up and running on your Mac.

**If you install Node.js using the installer on a Mac then it will likely lead to permissions errors later on and require you to use **`sudo`** way too much. Please use Homebrew to install Node.js on a Mac.**

## Testing Your Installation

Once you have completed  your node installation, open a terminal on Mac or Gitbash on Windows and run the following command.  You should see the latest stable version of node.

```
node --version
```

Installing node also installs npm so you can test for that now too.

```
npm --version
```

It's also nice to know where your code has been installed and you can find this out by running the 'which' command.

```
which node
which npm
```



