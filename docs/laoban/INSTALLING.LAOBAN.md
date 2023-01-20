
# Install `laoban`

Laoban has been tested on Windows and Linux. It probably works on a Mac but I don't have a Mac to test it on!

## Pre-requisites

* Laoban requires [npm / node](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm) to operate.
  * [Installation instructions here](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
  * I develop on an old version of node (v14.21.2) because I want this to work with legacy systems. 
  * I have tested it on later versions of node (v19.x) and it works fine.
* If you are working with mono-repos `yarn` is strongly recommended over npm. 
  * [Installing yarn instructions here](https://classic.yarnpkg.com/lang/en/docs/install/#windows-stable)
* If you are working with typescript you will need to have typescript installed globally
  * `npm install -g typescript`

## Installation

Laoban is a command line tool and is best installed from a command line

* It is typically installed by `npm i -g laoban` or (on Linux) `sudo npm i -g laoban`

## Privileges
It does require privileges to do this as it is adding an executable. I typically
run this command with either `sudo` on linux, or as administrator on Windows.

## Check laoban is installed

At the command line
```shell
laoban --version
```

## Problems

The only problems I have seen are related to permissions. If you get an error like this, it is
because you don't sufficient privileges to install the executable. If you have privileges, you
can use `sudo` on Linux or run as administrator on Windows. If you don't then you need to contact
your administrator

```shell
npm ERR! code EACCES    
``
